
Multi-agent
Multi-Agent Routing
Goal: multiple isolated agents (separate workspace + agentDir + sessions), plus multiple channel accounts (e.g. two WhatsApps) in one running Gateway. Inbound is routed to an agent via bindings.
​
What is “one agent”?
An agent is a fully scoped brain with its own:
Workspace (files, AGENTS.md/SOUL.md/USER.md, local notes, persona rules).
State directory (agentDir) for auth profiles, model registry, and per-agent config.
Session store (chat history + routing state) under ~/.openclaw/agents/<agentId>/sessions.
Auth profiles are per-agent. Each agent reads from its own:
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
Main agent credentials are not shared automatically. Never reuse agentDir across agents (it causes auth/session collisions). If you want to share creds, copy auth-profiles.json into the other agent’s agentDir.
Skills are per-agent via each workspace’s skills/ folder, with shared skills available from ~/.openclaw/skills. See Skills: per-agent vs shared.
The Gateway can host one agent (default) or many agents side-by-side.
Workspace note: each agent’s workspace is the default cwd, not a hard sandbox. Relative paths resolve inside the workspace, but absolute paths can reach other host locations unless sandboxing is enabled. See Sandboxing.
​
Paths (quick map)
Config: ~/.openclaw/openclaw.json (or OPENCLAW_CONFIG_PATH)
State dir: ~/.openclaw (or OPENCLAW_STATE_DIR)
Workspace: ~/.openclaw/workspace (or ~/.openclaw/workspace-<agentId>)
Agent dir: ~/.openclaw/agents/<agentId>/agent (or agents.list[].agentDir)
Sessions: ~/.openclaw/agents/<agentId>/sessions
​
Single-agent mode (default)
If you do nothing, OpenClaw runs a single agent:
agentId defaults to main.
Sessions are keyed as agent:main:<mainKey>.
Workspace defaults to ~/.openclaw/workspace (or ~/.openclaw/workspace-<profile> when OPENCLAW_PROFILE is set).
State defaults to ~/.openclaw/agents/main/agent.
​
Agent helper
Use the agent wizard to add a new isolated agent:
openclaw agents add work
Then add bindings (or let the wizard do it) to route inbound messages.
Verify with:
openclaw agents list --bindings
​
Quick start
1
Create each agent workspace

Use the wizard or create workspaces manually:
openclaw agents add coding
openclaw agents add social
Each agent gets its own workspace with SOUL.md, AGENTS.md, and optional USER.md, plus a dedicated agentDir and session store under ~/.openclaw/agents/<agentId>.
2
Create channel accounts

Create one account per agent on your preferred channels:
Discord: one bot per agent, enable Message Content Intent, copy each token.
Telegram: one bot per agent via BotFather, copy each token.
WhatsApp: link each phone number per account.
openclaw channels login --channel whatsapp --account work
See channel guides: Discord, Telegram, WhatsApp.
3
Add agents, accounts, and bindings

Add agents under agents.list, channel accounts under channels.<channel>.accounts, and connect them with bindings (examples below).
4
Restart and verify

openclaw gateway restart
openclaw agents list --bindings
openclaw channels status --probe
​
Multiple agents = multiple people, multiple personalities
With multiple agents, each agentId becomes a fully isolated persona:
Different phone numbers/accounts (per channel accountId).
Different personalities (per-agent workspace files like AGENTS.md and SOUL.md).
Separate auth + sessions (no cross-talk unless explicitly enabled).
This lets multiple people share one Gateway server while keeping their AI “brains” and data isolated.
​
One WhatsApp number, multiple people (DM split)
You can route different WhatsApp DMs to different agents while staying on one WhatsApp account. Match on sender E.164 (like +15551234567) with peer.kind: "direct". Replies still come from the same WhatsApp number (no per‑agent sender identity).
Important detail: direct chats collapse to the agent’s main session key, so true isolation requires one agent per person.
Example:
{
  agents: {
    list: [
      { id: "alex", workspace: "~/.openclaw/workspace-alex" },
      { id: "mia", workspace: "~/.openclaw/workspace-mia" },
    ],
  },
  bindings: [
    {
      agentId: "alex",
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230001" } },
    },
    {
      agentId: "mia",
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551230002" } },
    },
  ],
  channels: {
    whatsapp: {
      dmPolicy: "allowlist",
      allowFrom: ["+15551230001", "+15551230002"],
    },
  },
}
Notes:
DM access control is global per WhatsApp account (pairing/allowlist), not per agent.
For shared groups, bind the group to one agent or use Broadcast groups.
​
Routing rules (how messages pick an agent)
Bindings are deterministic and most-specific wins:
peer match (exact DM/group/channel id)
parentPeer match (thread inheritance)
guildId + roles (Discord role routing)
guildId (Discord)
teamId (Slack)
accountId match for a channel
channel-level match (accountId: "*")
fallback to default agent (agents.list[].default, else first list entry, default: main)
If multiple bindings match in the same tier, the first one in config order wins. If a binding sets multiple match fields (for example peer + guildId), all specified fields are required (AND semantics).
Important account-scope detail:
A binding that omits accountId matches the default account only.
Use accountId: "*" for a channel-wide fallback across all accounts.
If you later add the same binding for the same agent with an explicit account id, OpenClaw upgrades the existing channel-only binding to account-scoped instead of duplicating it.
​
Multiple accounts / phone numbers
Channels that support multiple accounts (e.g. WhatsApp) use accountId to identify each login. Each accountId can be routed to a different agent, so one server can host multiple phone numbers without mixing sessions.
If you want a channel-wide default account when accountId is omitted, set channels.<channel>.defaultAccount (optional). When unset, OpenClaw falls back to default if present, otherwise the first configured account id (sorted).
Common channels supporting this pattern include:
whatsapp, telegram, discord, slack, signal, imessage
irc, line, googlechat, mattermost, matrix, nextcloud-talk
bluebubbles, zalo, zalouser, nostr, feishu
​
Concepts
agentId: one “brain” (workspace, per-agent auth, per-agent session store).
accountId: one channel account instance (e.g. WhatsApp account "personal" vs "biz").
binding: routes inbound messages to an agentId by (channel, accountId, peer) and optionally guild/team ids.
Direct chats collapse to agent:<agentId>:<mainKey> (per-agent “main”; session.mainKey).
​
Platform examples
​
Discord bots per agent
Each Discord bot account maps to a unique accountId. Bind each account to an agent and keep allowlists per bot.
{
  agents: {
    list: [
      { id: "main", workspace: "~/.openclaw/workspace-main" },
      { id: "coding", workspace: "~/.openclaw/workspace-coding" },
    ],
  },
  bindings: [
    { agentId: "main", match: { channel: "discord", accountId: "default" } },
    { agentId: "coding", match: { channel: "discord", accountId: "coding" } },
  ],
  channels: {
    discord: {
      groupPolicy: "allowlist",
      accounts: {
        default: {
          token: "DISCORD_BOT_TOKEN_MAIN",
          guilds: {
            "123456789012345678": {
              channels: {
                "222222222222222222": { allow: true, requireMention: false },
              },
            },
          },
        },
        coding: {
          token: "DISCORD_BOT_TOKEN_CODING",
          guilds: {
            "123456789012345678": {
              channels: {
                "333333333333333333": { allow: true, requireMention: false },
              },
            },
          },
        },
      },
    },
  },
}
Notes:
Invite each bot to the guild and enable Message Content Intent.
Tokens live in channels.discord.accounts.<id>.token (default account can use DISCORD_BOT_TOKEN).
​
Telegram bots per agent
{
  agents: {
    list: [
      { id: "main", workspace: "~/.openclaw/workspace-main" },
      { id: "alerts", workspace: "~/.openclaw/workspace-alerts" },
    ],
  },
  bindings: [
    { agentId: "main", match: { channel: "telegram", accountId: "default" } },
    { agentId: "alerts", match: { channel: "telegram", accountId: "alerts" } },
  ],
  channels: {
    telegram: {
      accounts: {
        default: {
          botToken: "123456:ABC...",
          dmPolicy: "pairing",
        },
        alerts: {
          botToken: "987654:XYZ...",
          dmPolicy: "allowlist",
          allowFrom: ["tg:123456789"],
        },
      },
    },
  },
}
Notes:
Create one bot per agent with BotFather and copy each token.
Tokens live in channels.telegram.accounts.<id>.botToken (default account can use TELEGRAM_BOT_TOKEN).
​
WhatsApp numbers per agent
Link each account before starting the gateway:
openclaw channels login --channel whatsapp --account personal
openclaw channels login --channel whatsapp --account biz
~/.openclaw/openclaw.json (JSON5):
{
  agents: {
    list: [
      {
        id: "home",
        default: true,
        name: "Home",
        workspace: "~/.openclaw/workspace-home",
        agentDir: "~/.openclaw/agents/home/agent",
      },
      {
        id: "work",
        name: "Work",
        workspace: "~/.openclaw/workspace-work",
        agentDir: "~/.openclaw/agents/work/agent",
      },
    ],
  },

  // Deterministic routing: first match wins (most-specific first).
  bindings: [
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },

    // Optional per-peer override (example: send a specific group to work agent).
    {
      agentId: "work",
      match: {
        channel: "whatsapp",
        accountId: "personal",
        peer: { kind: "group", id: "1203630...@g.us" },
      },
    },
  ],

  // Off by default: agent-to-agent messaging must be explicitly enabled + allowlisted.
  tools: {
    agentToAgent: {
      enabled: false,
      allow: ["home", "work"],
    },
  },

  channels: {
    whatsapp: {
      accounts: {
        personal: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/personal
          // authDir: "~/.openclaw/credentials/whatsapp/personal",
        },
        biz: {
          // Optional override. Default: ~/.openclaw/credentials/whatsapp/biz
          // authDir: "~/.openclaw/credentials/whatsapp/biz",
        },
      },
    },
  },
}
​
Example: WhatsApp daily chat + Telegram deep work
Split by channel: route WhatsApp to a fast everyday agent and Telegram to an Opus agent.
{
  agents: {
    list: [
      {
        id: "chat",
        name: "Everyday",
        workspace: "~/.openclaw/workspace-chat",
        model: "anthropic/claude-sonnet-4-5",
      },
      {
        id: "opus",
        name: "Deep Work",
        workspace: "~/.openclaw/workspace-opus",
        model: "anthropic/claude-opus-4-6",
      },
    ],
  },
  bindings: [
    { agentId: "chat", match: { channel: "whatsapp" } },
    { agentId: "opus", match: { channel: "telegram" } },
  ],
}
Notes:
If you have multiple accounts for a channel, add accountId to the binding (for example { channel: "whatsapp", accountId: "personal" }).
To route a single DM/group to Opus while keeping the rest on chat, add a match.peer binding for that peer; peer matches always win over channel-wide rules.
​
Example: same channel, one peer to Opus
Keep WhatsApp on the fast agent, but route one DM to Opus:
{
  agents: {
    list: [
      {
        id: "chat",
        name: "Everyday",
        workspace: "~/.openclaw/workspace-chat",
        model: "anthropic/claude-sonnet-4-5",
      },
      {
        id: "opus",
        name: "Deep Work",
        workspace: "~/.openclaw/workspace-opus",
        model: "anthropic/claude-opus-4-6",
      },
    ],
  },
  bindings: [
    {
      agentId: "opus",
      match: { channel: "whatsapp", peer: { kind: "direct", id: "+15551234567" } },
    },
    { agentId: "chat", match: { channel: "whatsapp" } },
  ],
}
Peer bindings always win, so keep them above the channel-wide rule.
​
Family agent bound to a WhatsApp group
Bind a dedicated family agent to a single WhatsApp group, with mention gating and a tighter tool policy:
{
  agents: {
    list: [
      {
        id: "family",
        name: "Family",
        workspace: "~/.openclaw/workspace-family",
        identity: { name: "Family Bot" },
        groupChat: {
          mentionPatterns: ["@family", "@familybot", "@Family Bot"],
        },
        sandbox: {
          mode: "all",
          scope: "agent",
        },
        tools: {
          allow: [
            "exec",
            "read",
            "sessions_list",
            "sessions_history",
            "sessions_send",
            "sessions_spawn",
            "session_status",
          ],
          deny: ["write", "edit", "apply_patch", "browser", "canvas", "nodes", "cron"],
        },
      },
    ],
  },
  bindings: [
    {
      agentId: "family",
      match: {
        channel: "whatsapp",
        peer: { kind: "group", id: "120363999999999999@g.us" },
      },
    },
  ],
}
Notes:
Tool allow/deny lists are tools, not skills. If a skill needs to run a binary, ensure exec is allowed and the binary exists in the sandbox.
For stricter gating, set agents.list[].groupChat.mentionPatterns and keep group allowlists enabled for the channel.
​
Per-Agent Sandbox and Tool Configuration
Starting with v2026.1.6, each agent can have its own sandbox and tool restrictions:
{
  agents: {
    list: [
      {
        id: "personal",
        workspace: "~/.openclaw/workspace-personal",
        sandbox: {
          mode: "off",  // No sandbox for personal agent
        },
        // No tool restrictions - all tools available
      },
      {
        id: "family",
        workspace: "~/.openclaw/workspace-family",
        sandbox: {
          mode: "all",     // Always sandboxed
          scope: "agent",  // One container per agent
          docker: {
            // Optional one-time setup after container creation
            setupCommand: "apt-get update && apt-get install -y git curl",
          },
        },
        tools: {
          allow: ["read"],                    // Only read tool
          deny: ["exec", "write", "edit", "apply_patch"],    // Deny others
        },
      },
    ],
  },
}
Note: setupCommand lives under sandbox.docker and runs once on container creation. Per-agent sandbox.docker.* overrides are ignored when the resolved scope is "shared".
Benefits:
Security isolation: Restrict tools for untrusted agents
Resource control: Sandbox specific agents while keeping others on host
Flexible policies: Different permissions per agent
Note: tools.elevated is global and sender-based; it is not configurable per agent. If you need per-agent boundaries, use agents.list[].tools to deny exec. For group targeting, use agents.list[].groupChat.mentionPatterns so @mentions map cleanly to the intended agent.
See Multi-Agent Sandbox & Tools for detailed examples.


Multi-agent
Presence
OpenClaw “presence” is a lightweight, best‑effort view of:
the Gateway itself, and
clients connected to the Gateway (mac app, WebChat, CLI, etc.)
Presence is used primarily to render the macOS app’s Instances tab and to provide quick operator visibility.
​
Presence fields (what shows up)
Presence entries are structured objects with fields like:
instanceId (optional but strongly recommended): stable client identity (usually connect.client.instanceId)
host: human‑friendly host name
ip: best‑effort IP address
version: client version string
deviceFamily / modelIdentifier: hardware hints
mode: ui, webchat, cli, backend, probe, test, node, …
lastInputSeconds: “seconds since last user input” (if known)
reason: self, connect, node-connected, periodic, …
ts: last update timestamp (ms since epoch)
​
Producers (where presence comes from)
Presence entries are produced by multiple sources and merged.
​
1) Gateway self entry
The Gateway always seeds a “self” entry at startup so UIs show the gateway host even before any clients connect.
​
2) WebSocket connect
Every WS client begins with a connect request. On successful handshake the Gateway upserts a presence entry for that connection.
​
Why one‑off CLI commands don’t show up
The CLI often connects for short, one‑off commands. To avoid spamming the Instances list, client.mode === "cli" is not turned into a presence entry.
​
3) system-event beacons
Clients can send richer periodic beacons via the system-event method. The mac app uses this to report host name, IP, and lastInputSeconds.
​
4) Node connects (role: node)
When a node connects over the Gateway WebSocket with role: node, the Gateway upserts a presence entry for that node (same flow as other WS clients).
​
Merge + dedupe rules (why instanceId matters)
Presence entries are stored in a single in‑memory map:
Entries are keyed by a presence key.
The best key is a stable instanceId (from connect.client.instanceId) that survives restarts.
Keys are case‑insensitive.
If a client reconnects without a stable instanceId, it may show up as a duplicate row.
​
TTL and bounded size
Presence is intentionally ephemeral:
TTL: entries older than 5 minutes are pruned
Max entries: 200 (oldest dropped first)
This keeps the list fresh and avoids unbounded memory growth.
​
Remote/tunnel caveat (loopback IPs)
When a client connects over an SSH tunnel / local port forward, the Gateway may see the remote address as 127.0.0.1. To avoid overwriting a good client‑reported IP, loopback remote addresses are ignored.
​
Consumers
​
macOS Instances tab
The macOS app renders the output of system-presence and applies a small status indicator (Active/Idle/Stale) based on the age of the last update.
​
Debugging tips
To see the raw list, call system-presence against the Gateway.
If you see duplicates:
confirm clients send a stable client.instanceId in the handshake
confirm periodic beacons use the same instanceId
check whether the connection‑derived entry is missing instanceId (duplicates are expected)

Agent coordination
Multi-Agent Sandbox & Tools
​
Overview
Each agent in a multi-agent setup can now have its own:
Sandbox configuration (agents.list[].sandbox overrides agents.defaults.sandbox)
Tool restrictions (tools.allow / tools.deny, plus agents.list[].tools)
This allows you to run multiple agents with different security profiles:
Personal assistant with full access
Family/work agents with restricted tools
Public-facing agents in sandboxes
setupCommand belongs under sandbox.docker (global or per-agent) and runs once when the container is created.
Auth is per-agent: each agent reads from its own agentDir auth store at:
~/.openclaw/agents/<agentId>/agent/auth-profiles.json
Credentials are not shared between agents. Never reuse agentDir across agents. If you want to share creds, copy auth-profiles.json into the other agent’s agentDir.
For how sandboxing behaves at runtime, see Sandboxing. For debugging “why is this blocked?”, see Sandbox vs Tool Policy vs Elevated and openclaw sandbox explain.
​
Configuration Examples
​
Example 1: Personal + Restricted Family Agent
{
  "agents": {
    "list": [
      {
        "id": "main",
        "default": true,
        "name": "Personal Assistant",
        "workspace": "~/.openclaw/workspace",
        "sandbox": { "mode": "off" }
      },
      {
        "id": "family",
        "name": "Family Bot",
        "workspace": "~/.openclaw/workspace-family",
        "sandbox": {
          "mode": "all",
          "scope": "agent"
        },
        "tools": {
          "allow": ["read"],
          "deny": ["exec", "write", "edit", "apply_patch", "process", "browser"]
        }
      }
    ]
  },
  "bindings": [
    {
      "agentId": "family",
      "match": {
        "provider": "whatsapp",
        "accountId": "*",
        "peer": {
          "kind": "group",
          "id": "120363424282127706@g.us"
        }
      }
    }
  ]
}
Result:
main agent: Runs on host, full tool access
family agent: Runs in Docker (one container per agent), only read tool
​
Example 2: Work Agent with Shared Sandbox
{
  "agents": {
    "list": [
      {
        "id": "personal",
        "workspace": "~/.openclaw/workspace-personal",
        "sandbox": { "mode": "off" }
      },
      {
        "id": "work",
        "workspace": "~/.openclaw/workspace-work",
        "sandbox": {
          "mode": "all",
          "scope": "shared",
          "workspaceRoot": "/tmp/work-sandboxes"
        },
        "tools": {
          "allow": ["read", "write", "apply_patch", "exec"],
          "deny": ["browser", "gateway", "discord"]
        }
      }
    ]
  }
}
​
Example 2b: Global coding profile + messaging-only agent
{
  "tools": { "profile": "coding" },
  "agents": {
    "list": [
      {
        "id": "support",
        "tools": { "profile": "messaging", "allow": ["slack"] }
      }
    ]
  }
}
Result:
default agents get coding tools
support agent is messaging-only (+ Slack tool)
​
Example 3: Different Sandbox Modes per Agent
{
  "agents": {
    "defaults": {
      "sandbox": {
        "mode": "non-main", // Global default
        "scope": "session"
      }
    },
    "list": [
      {
        "id": "main",
        "workspace": "~/.openclaw/workspace",
        "sandbox": {
          "mode": "off" // Override: main never sandboxed
        }
      },
      {
        "id": "public",
        "workspace": "~/.openclaw/workspace-public",
        "sandbox": {
          "mode": "all", // Override: public always sandboxed
          "scope": "agent"
        },
        "tools": {
          "allow": ["read"],
          "deny": ["exec", "write", "edit", "apply_patch"]
        }
      }
    ]
  }
}
​
Configuration Precedence
When both global (agents.defaults.*) and agent-specific (agents.list[].*) configs exist:
​
Sandbox Config
Agent-specific settings override global:
agents.list[].sandbox.mode > agents.defaults.sandbox.mode
agents.list[].sandbox.scope > agents.defaults.sandbox.scope
agents.list[].sandbox.workspaceRoot > agents.defaults.sandbox.workspaceRoot
agents.list[].sandbox.workspaceAccess > agents.defaults.sandbox.workspaceAccess
agents.list[].sandbox.docker.* > agents.defaults.sandbox.docker.*
agents.list[].sandbox.browser.* > agents.defaults.sandbox.browser.*
agents.list[].sandbox.prune.* > agents.defaults.sandbox.prune.*
Notes:
agents.list[].sandbox.{docker,browser,prune}.* overrides agents.defaults.sandbox.{docker,browser,prune}.* for that agent (ignored when sandbox scope resolves to "shared").
​
Tool Restrictions
The filtering order is:
Tool profile (tools.profile or agents.list[].tools.profile)
Provider tool profile (tools.byProvider[provider].profile or agents.list[].tools.byProvider[provider].profile)
Global tool policy (tools.allow / tools.deny)
Provider tool policy (tools.byProvider[provider].allow/deny)
Agent-specific tool policy (agents.list[].tools.allow/deny)
Agent provider policy (agents.list[].tools.byProvider[provider].allow/deny)
Sandbox tool policy (tools.sandbox.tools or agents.list[].tools.sandbox.tools)
Subagent tool policy (tools.subagents.tools, if applicable)
Each level can further restrict tools, but cannot grant back denied tools from earlier levels. If agents.list[].tools.sandbox.tools is set, it replaces tools.sandbox.tools for that agent. If agents.list[].tools.profile is set, it overrides tools.profile for that agent. Provider tool keys accept either provider (e.g. google-antigravity) or provider/model (e.g. openai/gpt-5.2).
​
Tool groups (shorthands)
Tool policies (global, agent, sandbox) support group:* entries that expand to multiple concrete tools:
group:runtime: exec, bash, process
group:fs: read, write, edit, apply_patch
group:sessions: sessions_list, sessions_history, sessions_send, sessions_spawn, session_status
group:memory: memory_search, memory_get
group:ui: browser, canvas
group:automation: cron, gateway
group:messaging: message
group:nodes: nodes
group:openclaw: all built-in OpenClaw tools (excludes provider plugins)
​
Elevated Mode
tools.elevated is the global baseline (sender-based allowlist). agents.list[].tools.elevated can further restrict elevated for specific agents (both must allow).
Mitigation patterns:
Deny exec for untrusted agents (agents.list[].tools.deny: ["exec"])
Avoid allowlisting senders that route to restricted agents
Disable elevated globally (tools.elevated.enabled: false) if you only want sandboxed execution
Disable elevated per agent (agents.list[].tools.elevated.enabled: false) for sensitive profiles
​
Migration from Single Agent
Before (single agent):
{
  "agents": {
    "defaults": {
      "workspace": "~/.openclaw/workspace",
      "sandbox": {
        "mode": "non-main"
      }
    }
  },
  "tools": {
    "sandbox": {
      "tools": {
        "allow": ["read", "write", "apply_patch", "exec"],
        "deny": []
      }
    }
  }
}
After (multi-agent with different profiles):
{
  "agents": {
    "list": [
      {
        "id": "main",
        "default": true,
        "workspace": "~/.openclaw/workspace",
        "sandbox": { "mode": "off" }
      }
    ]
  }
}
Legacy agent.* configs are migrated by openclaw doctor; prefer agents.defaults + agents.list going forward.
​
Tool Restriction Examples
​
Read-only Agent
{
  "tools": {
    "allow": ["read"],
    "deny": ["exec", "write", "edit", "apply_patch", "process"]
  }
}
​
Safe Execution Agent (no file modifications)
{
  "tools": {
    "allow": ["read", "exec", "process"],
    "deny": ["write", "edit", "apply_patch", "browser", "gateway"]
  }
}
​
Communication-only Agent
{
  "tools": {
    "sessions": { "visibility": "tree" },
    "allow": ["sessions_list", "sessions_send", "sessions_history", "session_status"],
    "deny": ["exec", "write", "edit", "apply_patch", "read", "browser"]
  }
}
​
Common Pitfall: “non-main”
agents.defaults.sandbox.mode: "non-main" is based on session.mainKey (default "main"), not the agent id. Group/channel sessions always get their own keys, so they are treated as non-main and will be sandboxed. If you want an agent to never sandbox, set agents.list[].sandbox.mode: "off".
​
Testing
After configuring multi-agent sandbox and tools:
Check agent resolution:
openclaw agents list --bindings
Verify sandbox containers:
docker ps --filter "name=openclaw-sbx-"
Test tool restrictions:
Send a message requiring restricted tools
Verify the agent cannot use denied tools
Monitor logs:
tail -f "${OPENCLAW_STATE_DIR:-$HOME/.openclaw}/logs/gateway.log" | grep -E "routing|sandbox|tools"
​
Troubleshooting
​
Agent not sandboxed despite mode: "all"
Check if there’s a global agents.defaults.sandbox.mode that overrides it
Agent-specific config takes precedence, so set agents.list[].sandbox.mode: "all"
​
Tools still available despite deny list
Check tool filtering order: global → agent → sandbox → subagent
Each level can only further restrict, not grant back
Verify with logs: [tools] filtering tools for agent:${agentId}
​
Container not isolated per agent
Set scope: "agent" in agent-specific sandbox config
Default is "session" which creates one container per session
​
See Also
Multi-Agent Routing
Sandbox Configuration
Session Management

Agent coordination
ACP Agents
Agent Client Protocol (ACP) sessions let OpenClaw run external coding harnesses (for example Pi, Claude Code, Codex, OpenCode, and Gemini CLI) through an ACP backend plugin.
If you ask OpenClaw in plain language to “run this in Codex” or “start Claude Code in a thread”, OpenClaw should route that request to the ACP runtime (not the native sub-agent runtime).
​
Fast operator flow
Use this when you want a practical /acp runbook:
Spawn a session:
/acp spawn codex --mode persistent --thread auto
Work in the bound thread (or target that session key explicitly).
Check runtime state:
/acp status
Tune runtime options as needed:
/acp model <provider/model>
/acp permissions <profile>
/acp timeout <seconds>
Nudge an active session without replacing context:
/acp steer tighten logging and continue
Stop work:
/acp cancel (stop current turn), or
/acp close (close session + remove bindings)
​
Quick start for humans
Examples of natural requests:
“Start a persistent Codex session in a thread here and keep it focused.”
“Run this as a one-shot Claude Code ACP session and summarize the result.”
“Use Gemini CLI for this task in a thread, then keep follow-ups in that same thread.”
What OpenClaw should do:
Pick runtime: "acp".
Resolve the requested harness target (agentId, for example codex).
If thread binding is requested and the current channel supports it, bind the ACP session to the thread.
Route follow-up thread messages to that same ACP session until unfocused/closed/expired.
​
ACP versus sub-agents
Use ACP when you want an external harness runtime. Use sub-agents when you want OpenClaw-native delegated runs.
Area	ACP session	Sub-agent run
Runtime	ACP backend plugin (for example acpx)	OpenClaw native sub-agent runtime
Session key	agent:<agentId>:acp:<uuid>	agent:<agentId>:subagent:<uuid>
Main commands	/acp ...	/subagents ...
Spawn tool	sessions_spawn with runtime:"acp"	sessions_spawn (default runtime)
See also Sub-agents.
​
Thread-bound sessions (channel-agnostic)
When thread bindings are enabled for a channel adapter, ACP sessions can be bound to threads:
OpenClaw binds a thread to a target ACP session.
Follow-up messages in that thread route to the bound ACP session.
ACP output is delivered back to the same thread.
Unfocus/close/archive/idle-timeout or max-age expiry removes the binding.
Thread binding support is adapter-specific. If the active channel adapter does not support thread bindings, OpenClaw returns a clear unsupported/unavailable message.
Required feature flags for thread-bound ACP:
acp.enabled=true
acp.dispatch.enabled is on by default (set false to pause ACP dispatch)
Channel-adapter ACP thread-spawn flag enabled (adapter-specific)
Discord: channels.discord.threadBindings.spawnAcpSessions=true
Telegram: channels.telegram.threadBindings.spawnAcpSessions=true
​
Thread supporting channels
Any channel adapter that exposes session/thread binding capability.
Current built-in support:
Discord threads/channels
Telegram topics (forum topics in groups/supergroups and DM topics)
Plugin channels can add support through the same binding interface.
​
Channel specific settings
For non-ephemeral workflows, configure persistent ACP bindings in top-level bindings[] entries.
​
Binding model
bindings[].type="acp" marks a persistent ACP conversation binding.
bindings[].match identifies the target conversation:
Discord channel or thread: match.channel="discord" + match.peer.id="<channelOrThreadId>"
Telegram forum topic: match.channel="telegram" + match.peer.id="<chatId>:topic:<topicId>"
bindings[].agentId is the owning OpenClaw agent id.
Optional ACP overrides live under bindings[].acp:
mode (persistent or oneshot)
label
cwd
backend
​
Runtime defaults per agent
Use agents.list[].runtime to define ACP defaults once per agent:
agents.list[].runtime.type="acp"
agents.list[].runtime.acp.agent (harness id, for example codex or claude)
agents.list[].runtime.acp.backend
agents.list[].runtime.acp.mode
agents.list[].runtime.acp.cwd
Override precedence for ACP bound sessions:
bindings[].acp.*
agents.list[].runtime.acp.*
global ACP defaults (for example acp.backend)
Example:
{
  agents: {
    list: [
      {
        id: "codex",
        runtime: {
          type: "acp",
          acp: {
            agent: "codex",
            backend: "acpx",
            mode: "persistent",
            cwd: "/workspace/openclaw",
          },
        },
      },
      {
        id: "claude",
        runtime: {
          type: "acp",
          acp: { agent: "claude", backend: "acpx", mode: "persistent" },
        },
      },
    ],
  },
  bindings: [
    {
      type: "acp",
      agentId: "codex",
      match: {
        channel: "discord",
        accountId: "default",
        peer: { kind: "channel", id: "222222222222222222" },
      },
      acp: { label: "codex-main" },
    },
    {
      type: "acp",
      agentId: "claude",
      match: {
        channel: "telegram",
        accountId: "default",
        peer: { kind: "group", id: "-1001234567890:topic:42" },
      },
      acp: { cwd: "/workspace/repo-b" },
    },
    {
      type: "route",
      agentId: "main",
      match: { channel: "discord", accountId: "default" },
    },
    {
      type: "route",
      agentId: "main",
      match: { channel: "telegram", accountId: "default" },
    },
  ],
  channels: {
    discord: {
      guilds: {
        "111111111111111111": {
          channels: {
            "222222222222222222": { requireMention: false },
          },
        },
      },
    },
    telegram: {
      groups: {
        "-1001234567890": {
          topics: { "42": { requireMention: false } },
        },
      },
    },
  },
}
Behavior:
OpenClaw ensures the configured ACP session exists before use.
Messages in that channel or topic route to the configured ACP session.
In bound conversations, /new and /reset reset the same ACP session key in place.
Temporary runtime bindings (for example created by thread-focus flows) still apply where present.
​
Start ACP sessions (interfaces)
​
From sessions_spawn
Use runtime: "acp" to start an ACP session from an agent turn or tool call.
{
  "task": "Open the repo and summarize failing tests",
  "runtime": "acp",
  "agentId": "codex",
  "thread": true,
  "mode": "session"
}
Notes:
runtime defaults to subagent, so set runtime: "acp" explicitly for ACP sessions.
If agentId is omitted, OpenClaw uses acp.defaultAgent when configured.
mode: "session" requires thread: true to keep a persistent bound conversation.
Interface details:
task (required): initial prompt sent to the ACP session.
runtime (required for ACP): must be "acp".
agentId (optional): ACP target harness id. Falls back to acp.defaultAgent if set.
thread (optional, default false): request thread binding flow where supported.
mode (optional): run (one-shot) or session (persistent).
default is run
if thread: true and mode omitted, OpenClaw may default to persistent behavior per runtime path
mode: "session" requires thread: true
cwd (optional): requested runtime working directory (validated by backend/runtime policy).
label (optional): operator-facing label used in session/banner text.
resumeSessionId (optional): resume an existing ACP session instead of creating a new one. The agent replays its conversation history via session/load. Requires runtime: "acp".
streamTo (optional): "parent" streams initial ACP run progress summaries back to the requester session as system events.
When available, accepted responses include streamLogPath pointing to a session-scoped JSONL log (<sessionId>.acp-stream.jsonl) you can tail for full relay history.
​
Resume an existing session
Use resumeSessionId to continue a previous ACP session instead of starting fresh. The agent replays its conversation history via session/load, so it picks up with full context of what came before.
{
  "task": "Continue where we left off — fix the remaining test failures",
  "runtime": "acp",
  "agentId": "codex",
  "resumeSessionId": "<previous-session-id>"
}
Common use cases:
Hand off a Codex session from your laptop to your phone — tell your agent to pick up where you left off
Continue a coding session you started interactively in the CLI, now headlessly through your agent
Pick up work that was interrupted by a gateway restart or idle timeout
Notes:
resumeSessionId requires runtime: "acp" — returns an error if used with the sub-agent runtime.
resumeSessionId restores the upstream ACP conversation history; thread and mode still apply normally to the new OpenClaw session you are creating, so mode: "session" still requires thread: true.
The target agent must support session/load (Codex and Claude Code do).
If the session ID isn’t found, the spawn fails with a clear error — no silent fallback to a new session.
​
Operator smoke test
Use this after a gateway deploy when you want a quick live check that ACP spawn is actually working end-to-end, not just passing unit tests.
Recommended gate:
Verify the deployed gateway version/commit on the target host.
Confirm the deployed source includes the ACP lineage acceptance in src/gateway/sessions-patch.ts (subagent:* or acp:* sessions).
Open a temporary ACPX bridge session to a live agent (for example razor(main) on jpclawhq).
Ask that agent to call sessions_spawn with:
runtime: "acp"
agentId: "codex"
mode: "run"
task: Reply with exactly LIVE-ACP-SPAWN-OK
Verify the agent reports:
accepted=yes
a real childSessionKey
no validator error
Clean up the temporary ACPX bridge session.
Example prompt to the live agent:
Use the sessions_spawn tool now with runtime: "acp", agentId: "codex", and mode: "run".
Set the task to: "Reply with exactly LIVE-ACP-SPAWN-OK".
Then report only: accepted=<yes/no>; childSessionKey=<value or none>; error=<exact text or none>.
Notes:
Keep this smoke test on mode: "run" unless you are intentionally testing thread-bound persistent ACP sessions.
Do not require streamTo: "parent" for the basic gate. That path depends on requester/session capabilities and is a separate integration check.
Treat thread-bound mode: "session" testing as a second, richer integration pass from a real Discord thread or Telegram topic.
​
Sandbox compatibility
ACP sessions currently run on the host runtime, not inside the OpenClaw sandbox.
Current limitations:
If the requester session is sandboxed, ACP spawns are blocked for both sessions_spawn({ runtime: "acp" }) and /acp spawn.
Error: Sandboxed sessions cannot spawn ACP sessions because runtime="acp" runs on the host. Use runtime="subagent" from sandboxed sessions.
sessions_spawn with runtime: "acp" does not support sandbox: "require".
Error: sessions_spawn sandbox="require" is unsupported for runtime="acp" because ACP sessions run outside the sandbox. Use runtime="subagent" or sandbox="inherit".
Use runtime: "subagent" when you need sandbox-enforced execution.
​
From /acp command
Use /acp spawn for explicit operator control from chat when needed.
/acp spawn codex --mode persistent --thread auto
/acp spawn codex --mode oneshot --thread off
/acp spawn codex --thread here
Key flags:
--mode persistent|oneshot
--thread auto|here|off
--cwd <absolute-path>
--label <name>
See Slash Commands.
​
Session target resolution
Most /acp actions accept an optional session target (session-key, session-id, or session-label).
Resolution order:
Explicit target argument (or --session for /acp steer)
tries key
then UUID-shaped session id
then label
Current thread binding (if this conversation/thread is bound to an ACP session)
Current requester session fallback
If no target resolves, OpenClaw returns a clear error (Unable to resolve session target: ...).
​
Spawn thread modes
/acp spawn supports --thread auto|here|off.
Mode	Behavior
auto	In an active thread: bind that thread. Outside a thread: create/bind a child thread when supported.
here	Require current active thread; fail if not in one.
off	No binding. Session starts unbound.
Notes:
On non-thread binding surfaces, default behavior is effectively off.
Thread-bound spawn requires channel policy support:
Discord: channels.discord.threadBindings.spawnAcpSessions=true
Telegram: channels.telegram.threadBindings.spawnAcpSessions=true
​
ACP controls
Available command family:
/acp spawn
/acp cancel
/acp steer
/acp close
/acp status
/acp set-mode
/acp set
/acp cwd
/acp permissions
/acp timeout
/acp model
/acp reset-options
/acp sessions
/acp doctor
/acp install
/acp status shows the effective runtime options and, when available, both runtime-level and backend-level session identifiers.
Some controls depend on backend capabilities. If a backend does not support a control, OpenClaw returns a clear unsupported-control error.
​
ACP command cookbook
Command	What it does	Example
/acp spawn	Create ACP session; optional thread bind.	/acp spawn codex --mode persistent --thread auto --cwd /repo
/acp cancel	Cancel in-flight turn for target session.	/acp cancel agent:codex:acp:<uuid>
/acp steer	Send steer instruction to running session.	/acp steer --session support inbox prioritize failing tests
/acp close	Close session and unbind thread targets.	/acp close
/acp status	Show backend, mode, state, runtime options, capabilities.	/acp status
/acp set-mode	Set runtime mode for target session.	/acp set-mode plan
/acp set	Generic runtime config option write.	/acp set model openai/gpt-5.2
/acp cwd	Set runtime working directory override.	/acp cwd /Users/user/Projects/repo
/acp permissions	Set approval policy profile.	/acp permissions strict
/acp timeout	Set runtime timeout (seconds).	/acp timeout 120
/acp model	Set runtime model override.	/acp model anthropic/claude-opus-4-5
/acp reset-options	Remove session runtime option overrides.	/acp reset-options
/acp sessions	List recent ACP sessions from store.	/acp sessions
/acp doctor	Backend health, capabilities, actionable fixes.	/acp doctor
/acp install	Print deterministic install and enable steps.	/acp install
​
Runtime options mapping
/acp has convenience commands and a generic setter.
Equivalent operations:
/acp model <id> maps to runtime config key model.
/acp permissions <profile> maps to runtime config key approval_policy.
/acp timeout <seconds> maps to runtime config key timeout.
/acp cwd <path> updates runtime cwd override directly.
/acp set <key> <value> is the generic path.
Special case: key=cwd uses the cwd override path.
/acp reset-options clears all runtime overrides for target session.
​
acpx harness support (current)
Current acpx built-in harness aliases:
pi
claude
codex
opencode
gemini
kimi
When OpenClaw uses the acpx backend, prefer these values for agentId unless your acpx config defines custom agent aliases.
Direct acpx CLI usage can also target arbitrary adapters via --agent <command>, but that raw escape hatch is an acpx CLI feature (not the normal OpenClaw agentId path).
​
Required config
Core ACP baseline:
{
  acp: {
    enabled: true,
    // Optional. Default is true; set false to pause ACP dispatch while keeping /acp controls.
    dispatch: { enabled: true },
    backend: "acpx",
    defaultAgent: "codex",
    allowedAgents: ["pi", "claude", "codex", "opencode", "gemini", "kimi"],
    maxConcurrentSessions: 8,
    stream: {
      coalesceIdleMs: 300,
      maxChunkChars: 1200,
    },
    runtime: {
      ttlMinutes: 120,
    },
  },
}
Thread binding config is channel-adapter specific. Example for Discord:
{
  session: {
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
  },
  channels: {
    discord: {
      threadBindings: {
        enabled: true,
        spawnAcpSessions: true,
      },
    },
  },
}
If thread-bound ACP spawn does not work, verify the adapter feature flag first:
Discord: channels.discord.threadBindings.spawnAcpSessions=true
See Configuration Reference.
​
Plugin setup for acpx backend
Install and enable plugin:
openclaw plugins install acpx
openclaw config set plugins.entries.acpx.enabled true
Local workspace install during development:
openclaw plugins install ./extensions/acpx
Then verify backend health:
/acp doctor
​
acpx command and version configuration
By default, the acpx plugin (published as @openclaw/acpx) uses the plugin-local pinned binary:
Command defaults to extensions/acpx/node_modules/.bin/acpx.
Expected version defaults to the extension pin.
Startup registers ACP backend immediately as not-ready.
A background ensure job verifies acpx --version.
If the plugin-local binary is missing or mismatched, it runs: npm install --omit=dev --no-save acpx@<pinned> and re-verifies.
You can override command/version in plugin config:
{
  "plugins": {
    "entries": {
      "acpx": {
        "enabled": true,
        "config": {
          "command": "../acpx/dist/cli.js",
          "expectedVersion": "any"
        }
      }
    }
  }
}
Notes:
command accepts an absolute path, relative path, or command name (acpx).
Relative paths resolve from OpenClaw workspace directory.
expectedVersion: "any" disables strict version matching.
When command points to a custom binary/path, plugin-local auto-install is disabled.
OpenClaw startup remains non-blocking while the backend health check runs.
See Plugins.
​
Permission configuration
ACP sessions run non-interactively — there is no TTY to approve or deny file-write and shell-exec permission prompts. The acpx plugin provides two config keys that control how permissions are handled:
​
permissionMode
Controls which operations the harness agent can perform without prompting.
Value	Behavior
approve-all	Auto-approve all file writes and shell commands.
approve-reads	Auto-approve reads only; writes and exec require prompts.
deny-all	Deny all permission prompts.
​
nonInteractivePermissions
Controls what happens when a permission prompt would be shown but no interactive TTY is available (which is always the case for ACP sessions).
Value	Behavior
fail	Abort the session with AcpRuntimeError. (default)
deny	Silently deny the permission and continue (graceful degradation).
​
Configuration
Set via plugin config:
openclaw config set plugins.entries.acpx.config.permissionMode approve-all
openclaw config set plugins.entries.acpx.config.nonInteractivePermissions fail
Restart the gateway after changing these values.
Important: OpenClaw currently defaults to permissionMode=approve-reads and nonInteractivePermissions=fail. In non-interactive ACP sessions, any write or exec that triggers a permission prompt can fail with AcpRuntimeError: Permission prompt unavailable in non-interactive mode. If you need to restrict permissions, set nonInteractivePermissions to deny so sessions degrade gracefully instead of crashing.
​
Troubleshooting
Symptom	Likely cause	Fix
ACP runtime backend is not configured	Backend plugin missing or disabled.	Install and enable backend plugin, then run /acp doctor.
ACP is disabled by policy (acp.enabled=false)	ACP globally disabled.	Set acp.enabled=true.
ACP dispatch is disabled by policy (acp.dispatch.enabled=false)	Dispatch from normal thread messages disabled.	Set acp.dispatch.enabled=true.
ACP agent "<id>" is not allowed by policy	Agent not in allowlist.	Use allowed agentId or update acp.allowedAgents.
Unable to resolve session target: ...	Bad key/id/label token.	Run /acp sessions, copy exact key/label, retry.
--thread here requires running /acp spawn inside an active ... thread	--thread here used outside a thread context.	Move to target thread or use --thread auto/off.
Only <user-id> can rebind this thread.	Another user owns thread binding.	Rebind as owner or use a different thread.
Thread bindings are unavailable for <channel>.	Adapter lacks thread binding capability.	Use --thread off or move to supported adapter/channel.
Sandboxed sessions cannot spawn ACP sessions ...	ACP runtime is host-side; requester session is sandboxed.	Use runtime="subagent" from sandboxed sessions, or run ACP spawn from a non-sandboxed session.
sessions_spawn sandbox="require" is unsupported for runtime="acp" ...	sandbox="require" requested for ACP runtime.	Use runtime="subagent" for required sandboxing, or use ACP with sandbox="inherit" from a non-sandboxed session.
Missing ACP metadata for bound session	Stale/deleted ACP session metadata.	Recreate with /acp spawn, then rebind/focus thread.
AcpRuntimeError: Permission prompt unavailable in non-interactive mode	permissionMode blocks writes/exec in non-interactive ACP session.	Set plugins.entries.acpx.config.permissionMode to approve-all and restart gateway. See Permission configuration.
ACP session fails early with little output	Permission prompts are blocked by permissionMode/nonInteractivePermissions.	Check gateway logs for AcpRuntimeError. For full permissions, set permissionMode=approve-all; for graceful degradation, set nonInteractivePermissions=deny.
ACP session stalls indefinitely after completing work	Harness process finished but ACP session did not report completion.	Monitor with ps aux | grep acpx; kill stale processes manually.

Agent coordination
Sub-Agents
Sub-agents are background agent runs spawned from an existing agent run. They run in their own session (agent:<agentId>:subagent:<uuid>) and, when finished, announce their result back to the requester chat channel.
​
Slash command
Use /subagents to inspect or control sub-agent runs for the current session:
/subagents list
/subagents kill <id|#|all>
/subagents log <id|#> [limit] [tools]
/subagents info <id|#>
/subagents send <id|#> <message>
/subagents steer <id|#> <message>
/subagents spawn <agentId> <task> [--model <model>] [--thinking <level>]
Thread binding controls:
These commands work on channels that support persistent thread bindings. See Thread supporting channels below.
/focus <subagent-label|session-key|session-id|session-label>
/unfocus
/agents
/session idle <duration|off>
/session max-age <duration|off>
/subagents info shows run metadata (status, timestamps, session id, transcript path, cleanup).
​
Spawn behavior
/subagents spawn starts a background sub-agent as a user command, not an internal relay, and it sends one final completion update back to the requester chat when the run finishes.
The spawn command is non-blocking; it returns a run id immediately.
On completion, the sub-agent announces a summary/result message back to the requester chat channel.
For manual spawns, delivery is resilient:
OpenClaw tries direct agent delivery first with a stable idempotency key.
If direct delivery fails, it falls back to queue routing.
If queue routing is still not available, the announce is retried with a short exponential backoff before final give-up.
The completion handoff to the requester session is runtime-generated internal context (not user-authored text) and includes:
Result (assistant reply text, or latest toolResult if the assistant reply is empty)
Status (completed successfully / failed / timed out / unknown)
compact runtime/token stats
a delivery instruction telling the requester agent to rewrite in normal assistant voice (not forward raw internal metadata)
--model and --thinking override defaults for that specific run.
Use info/log to inspect details and output after completion.
/subagents spawn is one-shot mode (mode: "run"). For persistent thread-bound sessions, use sessions_spawn with thread: true and mode: "session".
For ACP harness sessions (Codex, Claude Code, Gemini CLI), use sessions_spawn with runtime: "acp" and see ACP Agents.
Primary goals:
Parallelize “research / long task / slow tool” work without blocking the main run.
Keep sub-agents isolated by default (session separation + optional sandboxing).
Keep the tool surface hard to misuse: sub-agents do not get session tools by default.
Support configurable nesting depth for orchestrator patterns.
Cost note: each sub-agent has its own context and token usage. For heavy or repetitive tasks, set a cheaper model for sub-agents and keep your main agent on a higher-quality model. You can configure this via agents.defaults.subagents.model or per-agent overrides.
​
Tool
Use sessions_spawn:
Starts a sub-agent run (deliver: false, global lane: subagent)
Then runs an announce step and posts the announce reply to the requester chat channel
Default model: inherits the caller unless you set agents.defaults.subagents.model (or per-agent agents.list[].subagents.model); an explicit sessions_spawn.model still wins.
Default thinking: inherits the caller unless you set agents.defaults.subagents.thinking (or per-agent agents.list[].subagents.thinking); an explicit sessions_spawn.thinking still wins.
Default run timeout: if sessions_spawn.runTimeoutSeconds is omitted, OpenClaw uses agents.defaults.subagents.runTimeoutSeconds when set; otherwise it falls back to 0 (no timeout).
Tool params:
task (required)
label? (optional)
agentId? (optional; spawn under another agent id if allowed)
model? (optional; overrides the sub-agent model; invalid values are skipped and the sub-agent runs on the default model with a warning in the tool result)
thinking? (optional; overrides thinking level for the sub-agent run)
runTimeoutSeconds? (defaults to agents.defaults.subagents.runTimeoutSeconds when set, otherwise 0; when set, the sub-agent run is aborted after N seconds)
thread? (default false; when true, requests channel thread binding for this sub-agent session)
mode? (run|session)
default is run
if thread: true and mode omitted, default becomes session
mode: "session" requires thread: true
cleanup? (delete|keep, default keep)
sandbox? (inherit|require, default inherit; require rejects spawn unless target child runtime is sandboxed)
sessions_spawn does not accept channel-delivery params (target, channel, to, threadId, replyTo, transport). For delivery, use message/sessions_send from the spawned run.
​
Thread-bound sessions
When thread bindings are enabled for a channel, a sub-agent can stay bound to a thread so follow-up user messages in that thread keep routing to the same sub-agent session.
​
Thread supporting channels
Discord (currently the only supported channel): supports persistent thread-bound subagent sessions (sessions_spawn with thread: true), manual thread controls (/focus, /unfocus, /agents, /session idle, /session max-age), and adapter keys channels.discord.threadBindings.enabled, channels.discord.threadBindings.idleHours, channels.discord.threadBindings.maxAgeHours, and channels.discord.threadBindings.spawnSubagentSessions.
Quick flow:
Spawn with sessions_spawn using thread: true (and optionally mode: "session").
OpenClaw creates or binds a thread to that session target in the active channel.
Replies and follow-up messages in that thread route to the bound session.
Use /session idle to inspect/update inactivity auto-unfocus and /session max-age to control the hard cap.
Use /unfocus to detach manually.
Manual controls:
/focus <target> binds the current thread (or creates one) to a sub-agent/session target.
/unfocus removes the binding for the current bound thread.
/agents lists active runs and binding state (thread:<id> or unbound).
/session idle and /session max-age only work for focused bound threads.
Config switches:
Global default: session.threadBindings.enabled, session.threadBindings.idleHours, session.threadBindings.maxAgeHours
Channel override and spawn auto-bind keys are adapter-specific. See Thread supporting channels above.
See Configuration Reference and Slash commands for current adapter details.
Allowlist:
agents.list[].subagents.allowAgents: list of agent ids that can be targeted via agentId (["*"] to allow any). Default: only the requester agent.
Sandbox inheritance guard: if the requester session is sandboxed, sessions_spawn rejects targets that would run unsandboxed.
Discovery:
Use agents_list to see which agent ids are currently allowed for sessions_spawn.
Auto-archive:
Sub-agent sessions are automatically archived after agents.defaults.subagents.archiveAfterMinutes (default: 60).
Archive uses sessions.delete and renames the transcript to *.deleted.<timestamp> (same folder).
cleanup: "delete" archives immediately after announce (still keeps the transcript via rename).
Auto-archive is best-effort; pending timers are lost if the gateway restarts.
runTimeoutSeconds does not auto-archive; it only stops the run. The session remains until auto-archive.
Auto-archive applies equally to depth-1 and depth-2 sessions.
​
Nested Sub-Agents
By default, sub-agents cannot spawn their own sub-agents (maxSpawnDepth: 1). You can enable one level of nesting by setting maxSpawnDepth: 2, which allows the orchestrator pattern: main → orchestrator sub-agent → worker sub-sub-agents.
​
How to enable
{
  agents: {
    defaults: {
      subagents: {
        maxSpawnDepth: 2, // allow sub-agents to spawn children (default: 1)
        maxChildrenPerAgent: 5, // max active children per agent session (default: 5)
        maxConcurrent: 8, // global concurrency lane cap (default: 8)
        runTimeoutSeconds: 900, // default timeout for sessions_spawn when omitted (0 = no timeout)
      },
    },
  },
}
​
Depth levels
Depth	Session key shape	Role	Can spawn?
0	agent:<id>:main	Main agent	Always
1	agent:<id>:subagent:<uuid>	Sub-agent (orchestrator when depth 2 allowed)	Only if maxSpawnDepth >= 2
2	agent:<id>:subagent:<uuid>:subagent:<uuid>	Sub-sub-agent (leaf worker)	Never
​
Announce chain
Results flow back up the chain:
Depth-2 worker finishes → announces to its parent (depth-1 orchestrator)
Depth-1 orchestrator receives the announce, synthesizes results, finishes → announces to main
Main agent receives the announce and delivers to the user
Each level only sees announces from its direct children.
​
Tool policy by depth
Role and control scope are written into session metadata at spawn time. That keeps flat or restored session keys from accidentally regaining orchestrator privileges.
Depth 1 (orchestrator, when maxSpawnDepth >= 2): Gets sessions_spawn, subagents, sessions_list, sessions_history so it can manage its children. Other session/system tools remain denied.
Depth 1 (leaf, when maxSpawnDepth == 1): No session tools (current default behavior).
Depth 2 (leaf worker): No session tools — sessions_spawn is always denied at depth 2. Cannot spawn further children.
​
Per-agent spawn limit
Each agent session (at any depth) can have at most maxChildrenPerAgent (default: 5) active children at a time. This prevents runaway fan-out from a single orchestrator.
​
Cascade stop
Stopping a depth-1 orchestrator automatically stops all its depth-2 children:
/stop in the main chat stops all depth-1 agents and cascades to their depth-2 children.
/subagents kill <id> stops a specific sub-agent and cascades to its children.
/subagents kill all stops all sub-agents for the requester and cascades.
​
Authentication
Sub-agent auth is resolved by agent id, not by session type:
The sub-agent session key is agent:<agentId>:subagent:<uuid>.
The auth store is loaded from that agent’s agentDir.
The main agent’s auth profiles are merged in as a fallback; agent profiles override main profiles on conflicts.
Note: the merge is additive, so main profiles are always available as fallbacks. Fully isolated auth per agent is not supported yet.
​
Announce
Sub-agents report back via an announce step:
The announce step runs inside the sub-agent session (not the requester session).
If the sub-agent replies exactly ANNOUNCE_SKIP, nothing is posted.
Otherwise delivery depends on requester depth:
top-level requester sessions use a follow-up agent call with external delivery (deliver=true)
nested requester subagent sessions receive an internal follow-up injection (deliver=false) so the orchestrator can synthesize child results in-session
if a nested requester subagent session is gone, OpenClaw falls back to that session’s requester when available
Child completion aggregation is scoped to the current requester run when building nested completion findings, preventing stale prior-run child outputs from leaking into the current announce.
Announce replies preserve thread/topic routing when available on channel adapters.
Announce context is normalized to a stable internal event block:
source (subagent or cron)
child session key/id
announce type + task label
status line derived from runtime outcome (success, error, timeout, or unknown)
result content from the announce step (or (no output) if missing)
a follow-up instruction describing when to reply vs. stay silent
Status is not inferred from model output; it comes from runtime outcome signals.
Announce payloads include a stats line at the end (even when wrapped):
Runtime (e.g., runtime 5m12s)
Token usage (input/output/total)
Estimated cost when model pricing is configured (models.providers.*.models[].cost)
sessionKey, sessionId, and transcript path (so the main agent can fetch history via sessions_history or inspect the file on disk)
Internal metadata is meant for orchestration only; user-facing replies should be rewritten in normal assistant voice.
​
Tool Policy (sub-agent tools)
By default, sub-agents get all tools except session tools and system tools:
sessions_list
sessions_history
sessions_send
sessions_spawn
When maxSpawnDepth >= 2, depth-1 orchestrator sub-agents additionally receive sessions_spawn, subagents, sessions_list, and sessions_history so they can manage their children.
Override via config:
{
  agents: {
    defaults: {
      subagents: {
        maxConcurrent: 1,
      },
    },
  },
  tools: {
    subagents: {
      tools: {
        // deny wins
        deny: ["gateway", "cron"],
        // if allow is set, it becomes allow-only (deny still wins)
        // allow: ["read", "exec", "process"]
      },
    },
  },
}
​
Concurrency
Sub-agents use a dedicated in-process queue lane:
Lane name: subagent
Concurrency: agents.defaults.subagents.maxConcurrent (default 8)
​
Stopping
Sending /stop in the requester chat aborts the requester session and stops any active sub-agent runs spawned from it, cascading to nested children.
/subagents kill <id> stops a specific sub-agent and cascades to its children.
​
Limitations
Sub-agent announce is best-effort. If the gateway restarts, pending “announce back” work is lost.
Sub-agents still share the same gateway process resources; treat maxConcurrent as a safety valve.
sessions_spawn is always non-blocking: it returns { status: "accepted", runId, childSessionKey } immediately.
Sub-agent context only injects AGENTS.md + TOOLS.md (no SOUL.md, IDENTITY.md, USER.md, HEARTBEAT.md, or BOOTSTRAP.md).
Maximum nesting depth is 5 (maxSpawnDepth range: 1–5). Depth 2 is recommended for most use cases.
maxChildrenPerAgent caps active children per session (default: 5, range: 1–20).
Agent Send
ACP Agents
Powered by
