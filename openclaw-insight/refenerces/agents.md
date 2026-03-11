智能体运行器Agent Runner
Agent Runner是基于Pi Agent Core（作者：Mario Zechner）这个Agent框架基础之上扩展而来的Agent执行引擎，负责协调所有Agent交互的模型推理、工具执行和会话管理。Agent Runner 会动态拼装 system prompt：包含可用工具（tools）、技能（skills）、记忆（memory），然后追加 session 历史（从 .jsonl 文件读取）。

多 Agent 路由（Multi-Agent Routing）
openclaw支持多 Agent的创建和协作——一个 Gateway，控制无限的 Agent。可以让不同的聊天渠道/群组使用完全不同的 AI 助手实例，各自独立。




每个 Agent 都在 ~/.openclaw/agents/ 目录下有自己的独立文件夹。有自己的工作区、会话和记忆。工作 Agent 了解你的技术栈和项目；私人 Agent 了解你的习惯和日程，它们互不干扰。

频道映射在 config.json 中配置。向某个 Telegram 聊天发送消息，它会路由到工作 Agent；向另一个聊天发送，则路由到私人 Agent。同一个 Gateway，通过规则进行路由分发。

dmScope 控制着隔离级别。把它设置为 per-agent，每个 Agent 只能看到属于自己的对话。你可以扩展出一个通过心跳监控服务器的 Agent，一个解析信息源并将摘要存入专属 MEMORY.md 的研究 Agent，一个监控流动性池并随时提醒你机会的交易 Agent。所有的 Agent 都在同一个 Gateway 上运行。

对于Agent 之间怎么互相沟通和协作，OpenClaw 提供了一组会话工具，让不同的 Agent 可以互相传话、协作：

sessions_list：查看有哪些活跃会话
sessions_send：给另一个会话发消息
sessions_history：查看另一个会话的对话记录
sessions_spawn：创建新会话来委派任务
规则：如果有多个人访问同一个 Agent，必须将 dmScope 设置为 per-channel-peer。否则，来自不同用户的会话会被折叠成一个。Agent 会用别人对话里的信息来回复你。这是个默认行为，你需要手动修改。
智能体的内核：pi-mono
关于pi智能体框架的详细内容，我将在单独的章节展开介绍。

Pi Agent Core (@mariozechner/pi-agent-core) 是一个面向 Node.js 环境的轻量级Agent运行时库。其核心设计目标是提供一个最小化的、传输层无关（Transport-Agnostic）的执行环境，用于构建有状态的、能够处理复杂工具调用的AI应用。

与LangChain等全栈式框架不同，Pi Agent Core采取“微内核”设计策略，剥离了具体模型集成与向量数据库依赖，专注于Agent的控制流与通信协议抽象。

pi-mono 是一个 monorepo，里面包含多个包（例如 pi-agent-core、pi-coding-agent、pi-ai、pi-tui 等），Pi Agent Core（pi-agent-core） 是 pi-mono 的一个包，提供 agent loop、工具执行与消息类型等核心能力。OpenClaw还使用到了pi-mono中的其他的功能模块。

会话管理
会话（Session）是常见的Agent系统或者ChatBot类应用中的必备功能。OpenClaw利用了Pi Agnet Core自带的Session管理的能力，并在此基础上做了少量扩展：

OpenClaw使用Pi Agent Core原生接口，但自定义会话存储位置
实现了系统提示动态构建、工具策略过滤、沙盒路径重定向、多认证轮换、提供商标记化
在OpenClaw中，每个Session由唯一的一个Key来标识，每个Key对应独立的对话历史和状态。关于结构和用法前面已经介绍过了。

OpenClaw对于SessionKey的独特设计，使得其能够处理并发冲突：同一Session Key的消息排队处理，不同Session Key的消息可并行处理。

对于排队的情况，支持的模式有：

sequential：同一会话的消息串行处理，防止并发冲突
concurrent：允许同一会话的多个Agent并发运行
collect：防抖并批量处理快速连续的消息
Steer：将排队消息注入当前运行
Followup：等待当前回合结束后处理排队消息
Session Store用于维护会话元数据的键值映射，OpenClaw还会在这个文件中维护一个Token计数器。位置默认在~/.openclaw/agents/<agentId>/sessions/sessions.json

Transcript用于存储完整的对话历史，采用JSONL格式，默认位置在~/.openclaw/agents/<agentId>/sessions/<sessionId>.jsonl，Transcript的存储采用树形结构，每条记录有 id 和 parentId，支持分支对话。

Session Manager负责读写转录文件、重建模型上下文、处理压缩、维护会话树结构

重置策略

OpenClaw有以下几种重置Session的策略：

每日重置：默认凌晨4点本地时间重置
空闲重置：idleMinutes 滑动窗口，无活动超时后重置
手动重置：/new 或 /reset 命令触发
优先级：每日和空闲同时配置时，先到期的先生效
压缩与清理

通大多数压缩策略一样，OpenClaw支持Summary这样的一种压缩策略，即上下文接近限制时触发，总结历史对话。

此外，还有一种“内存刷新”的设计，即在会话接近自动压缩时触发的一个后台内存整理机制，确保重要信息在压缩前被持久化保存。

具体来说，当会话token使用量接近上下文窗口限制时，Gateway会：

构建系统提示词
运行一个静默的agent回合
模型根据提示词判断并写入重要信息，写入memory/YYYY-MM-DD.md或MEMORY.md
使用 NO_REPLY 标记避免用户看到输出
下面是对于模型要提取的”重要信息“的定义：

### 3.2 What to Preserve

Preserve **specific details** that future-you will need:

| Preserve                     | Example                                                  |
| ---------------------------- | -------------------------------------------------------- |
| **Specific locations**       | "src/auth/login.ts:67" not "the auth code"               |
| **Exact findings**           | "SQL injection in query builder" not "security issues"   |
| **Decisions with rationale** | "Approved because X" not just "Approved"                 |
| **Numbers and thresholds**   | "Coverage at 73%, target is 80%" not "coverage is low"   |
| **Names and identifiers**    | "User.authenticate() method" not "the login function"    |
| **Open questions**           | "Need to verify: does rate limiter apply to OAuth flow?" |
上下文构建
上下文由系统提示词、Skills、对话历史，和BootstrapContext构成。

其中对话历史根据会话类型裁剪：DM会话使用session.dmHistoryLimit配置（默认无限制），群组会话使用session.historyLimit（默认 100 轮）；支持按通道覆盖配置 。

BootstrapContext则是通过resolveBootstrapFilesForRun方法来加载这些文件：AGENTS.md、SOUL.md、TOOLS.md、IDENTITY.md、USER.md、HEARTBEAT.md、BOOTSTRAP.md。然后调用 buildBootstrapContextFiles将这些文件按agents.defaults.bootstrapMaxChars（默认 20000）截断，生成 contextFiles用于注入到系统提示。

这几个被注入的Markdown文件来自Workspace 的一组 .md 文件，每个文件都有独特的作用，而且易于读写：

AGENTS.md：操作手册。Agent 应该如何思考，何时使用哪个工具，遵循什么安全规则，按什么顺序做事。
SOUL.md：性格与灵魂。语气、边界、优先级。希望 Agent 简洁明了不给多余建议？写在这里。想要一个友好的助手？也写在这里。
USER.md：你的用户画像。如何称呼你，你的职业，你的偏好。Agent 在每次回复前都会读取这个文件。
MEMORY.md：长期记忆。绝不能丢失的事实。比如“我们只在 DEX 上交易，不用 CEX”、“主 RPC 是 Alchemy，备用是 Infura”。Agent 会自行写入，或者在你要求时写入。
YYYY-MM-DD.md：每日日志。今天发生了什么，哪些任务正在进行，你们讨论了什么。到了明天，Agent 会打开昨天的日志并接续上下文。
BOOTSTRAP.md：首次运行仪式（一次性，仅全新工作空间注入），如引导对话等
IDENTITY.md：身份与氛围。很短的文件，但它奠定了整体的基调。
HEARTBEAT.md：定期检查清单。“检查邮件”、“看看监控是否在运行”。
TOOLS.md：本地工具提示。脚本存放在哪里，哪些命令可用。这样 Agent 就不需要去猜，而是确切知道。
值得一提的是，OpenClaw采用2种持久化的实体文件（MEMORY.md 与 memory/YYYY-MM-DD.md）来存储长期记忆，会话开始时会读取 MEMORY.md（仅主会话）和当日/昨日的日志文件作为初始上下文，运行中可通过memory_search 工具将相关片段动态拉入上下文。

当模型被提示“记住”时会写入这些文件，且系统在接近压缩阈值时会触发“内存刷新”时也会写入。

两层记忆的协同

每次运行时，Gateway 都会抓取 AGENTS.md、SOUL.md、USER.md、IDENTITY.md 以及当天的每日日志，在 LLM 看到你的消息之前将它们注入到上下文中。

这就是第一层：Bootstrap（引导记忆）。Agent 每次都能看到这些文件的内容，没有例外。但它们会消耗 Token。你在 Bootstrap 文件里塞得越多，每次请求就越贵。

第二层：Semantic search（语义搜索）。当启用记忆插件时，Agent 会通过向量索引搜索 MEMORY.md 和其他笔记，通过“含义”而不是“关键词”来寻找相关的块。你问“我们在哪个 DEX 上交易？”，它能找到正确的答案，即使那是你两个月前写的。

区别在于：

Bootstrap 是 Agent 每次必看的内容。
语义搜索 只在需要时提取当前相关的内容，不会持续消耗上下文，但它不能保证每次都能浮现出完全正确的事实。
在实际使用中，Bootstrap 层的膨胀速度往往会被忽视。一旦 AGENTS.md、USER.md、每日日志和会话历史开始累积，Gateway 最终会为每一次调用组装非常庞大的上下文。这在早期很好用，但长期运行的 Agent 可能会消耗惊人的 Token。
这指出了目前许多 Agent 运行时面临的一个核心设计问题：有多少状态应该存在于 Prompt 上下文中，又有多少应该存在于运行时系统本身？ OpenClaw 严重依赖基于 Prompt 的状态，这保持了一切的透明度和可编辑性，但也意味着一旦 Agent 运行了几个星期或几个月，上下文管理就会变成一个真正的工程问题。因此，合理规划两层记忆的使用，不仅是为了成本，更是为了保证系统长期的可用性。
大模型配置Model Provider
OpenClaw的Model Provider体系通过“内置目录 + 自定义配置 + 多认证轮换 + 模型降级”实现多提供商统一接入。最新版还支持配置备用大模型，以应对异常或超时的情况，考虑很周到。

OpenClaw随Pi Agent Core内置大量模型供应商（anthropic/openai/openai-codex/google/opencode/openrouter 等），无需额外配置。

所谓认证轮换，就是为每个模型供应商维护多个认证凭据（OAuth/API key），在单个凭据失败（如 401/402/429）时自动切换到下一个可用凭据，从而保证服务连续性、平滑应对速率限制与计费问题，并支持多租户/多账户隔离。

当前模型供应商的所有认证配置均失败、冷却或被禁用，且已配置了fallbacks列表（provider/model列表），则OpenClaw会自动切换至fallbacks列表中的下一个模型，此为模型降级机制。

关于多种认证的方式，有意思的是OpenClaw对不同厂商进行了简单的逆向，使得用户即便在不走API（使用API Key）的情况下，仅仅通过订阅账户的登录认证令牌，就能访问该厂商的模型服务。

以OpenAI为例，OpenClaw支持以下3种认证方式：

OpenAI Codex OAuth (Codex CLI)
OpenAI Codex (ChatGPT OAuth)
OpenAI API key
其中，Codex OAuth这种认证方式，如果用户曾经登录并认证过，会将凭证数据存储在macOS keychain中（如果是Mac用户），如果取不到，则从~/.codex/auth.json中获取，最终使用和API Key一样的方式调用https://api.openai.com/v1/chat/completions，并在Authentication头中，通过Bearer xxxx的方式完成调用。

而选择OpenAI Codex (ChatGPT OAuth)这种方式，会交互式的访问https://auth.openai.com/oauth/authorize、https://auth.openai.com/oauth/token等，最终拿到token、account_id等信息，然后在chat过程中调用https://chatgpt.com/backend-api/codex/responses，在Authentication头中则使用Authorization: Bearer 、chatgpt-account-id: 、OpenAI-Beta: responses=experimental

工具调用
工具Tool的调用主体是交给Pi Agent Core完成的，其内部仍然是经典的“tool_call → 执行 → tool_result→ 继续生成”的循环。不过Tool执行过程中所产生的start/update/result 事件，通过WebSocket事件流推送给前端或控制面，用于显示工具卡片、输出摘要、部分结果等。工具执行完毕后，结果被写回会话历史，随后模型继续生成最终回答，最终再通过同一条 WS 会话发回。

而OpenClaw在将可被调用的Tools交给Pi Agent Core之前，通过Tool Policy Resolver按allow/deny与分组策略裁剪可用工具，这一步决定了模型看到的工具表，并为后续执行建立安全边界。

命令执行

exec 是其中最强大的工具，它可以运行 Shell 命令。Agent 可以运行脚本、安装包、处理文件、部署代码。但它也是最危险的。exec 有三种模式：

sandbox：在 Docker 容器内运行 Agent，与主系统隔离。
gateway：直接在服务器上运行，但受限于你定义的白名单命令。
full：无限制。适合用来做实验，但绝对不适合放了真实数据的生产服务器。
openclaw会在你自担风险的前提下给 agent 很大的电脑访问权限，并通过 exec 工具执行 shell 命令，执行环境包括：

sandbox：默认，在 Docker 容器里运行命令
直接在宿主机（host machine）运行
在远程设备（remote devices）上运行
浏览器调用

browser 工具控制浏览器。打开网页、点击元素、打字、截图、保存 PDF。

定时器

Cron 是将 Agent 从“聊天机器人”变成“数字员工”的关键。设置一次定时任务：

openclaw cron add --schedule "0 9 * * *" --agent personal --prompt "检查新邮件，发送摘要到 Telegram" --announce
每天早上 9:00，Agent 醒来，执行任务，并将结果发送到频道，完全不需要你插手。--announce 标志负责将结果发送到频道，--no-deliver 则会静默运行不发送结果。

**Heartbeat（心跳）**则是根据 HEARTBEAT.md 进行的更频繁的定期检查。监控在运行吗？日志里有报错吗？如果出问题了，Agent 会主动给你发消息。

举例：假设你想每天早上检查 Gmail 并将摘要发到 Telegram。启用 browser 工具，设置 9:00 的 cron，在 AGENTS.md 中写好指令。每天早上 Agent 都会打开带有已保存会话的浏览器，读取收件箱，过滤发件人，并把摘要发给你。你咖啡还没喝完，事情就已经处理妥当了。
Agent Pi 分析—— 驱动 OpenClaw 的极简工程 Agent


Pi 的 Agent 是 OpenClaw 的引擎。 Pi 是一个只有四个工具、系统提示词不到 1000 token、信奉"删减比添加更重要"的 AI 编程 Agent，与其他编程 Agent 完全不同的极简工程和决策机制。

Pi 位于 badlogic/pi-mono，一个使用 npm workspaces 和锁定版本控制的 TypeScript monorepo。依赖图严格执行分层架构：

┌─────────────────────────────────────────┐
│  Applications                           │
│  pi-coding-agent, pi-tui, pi-mom, ...   │
├─────────────────────────────────────────┤
│  Core                                   │
│  pi-agent-core                          │
├─────────────────────────────────────────┤
│  Foundation                             │
│  pi-ai                                  │
└─────────────────────────────────────────┘
基础包零内部依赖。核心包仅依赖基础层。应用层在最上方。 这不是愿望——构建系统强制执行。

要理解Pi的设计逻辑，首先要跳出当前Agent开发的固有思维定式，读懂它的反直觉立场。在当前的Agent框架生态中，无论是Claude Code这样的巨头产品，还是各类开源项目，几乎都在沿着"做加法"的路径前进。大家普遍认为，Agent的能力越强，需要的工具就越多，系统提示词就越长，规划链路就越复杂，甚至需要引入多个子Agent协同工作，才能应对各类复杂场景。这种思路看似合理，却忽略了一个核心事实，那就是前沿的LLM模型经过RL训练后，已经具备了足够强的理解能力和执行能力，能够清晰地认知"编码Agent"的核心职责，根本不需要冗长的系统提示词和复杂的辅助模块来"教"它如何工作。

Mario Zechner对此有着明确的观点，他认为前沿模型已经被RL训练得足够理解"编码Agent"是什么，你不需要10000 token的系统提示词。这句话并非空谈，而是有着实打实的实践支撑。Pi在Terminal-Bench 2.0基准测试中，使用Claude Opus 4.5模型，成功跻身排行榜前列，与Codex、Cursor、Windsurf等拥有复杂工具链的Agent展开激烈竞争。而令人震惊的是，Pi的系统提示词加上工具定义的总长度还不到1000 token，不足Claude Code的十分之一，内置工具也只有4个，远远少于同类产品。

为了更清晰地展现Pi与主流Agent的差异，我们可以通过一组关键维度的对比来直观感受它的极简特性。

对比维度	Claude Code	Codex	Pi
系统提示词	约10000+ tokens	适中	< 1000 tokens
内置工具数	数十个	适中	4个 (read/write/edit/bash)
Plan Mode	有（黑盒子Agent）	有	无（用文件代替）
MCP 支持	有	有	无（用CLI工具代替）
Sub-Agent	有（不可观测）	---	无（通过bash自我调用）
从这份对比中我们可以发现，Pi在所有主流Agent都在强化的模块上几乎都做了减法。没有冗长的系统提示词，没有数量众多的内置工具，没有复杂的Plan Mode和MCP支持，更没有不可观测的子Agent。但就是这样一款"极简"的架构，却能在核心性能上与主流产品比肩，这背后恰恰体现了Pi设计思路的先进性------放弃冗余的辅助模块，让LLM回归核心，用最简洁的结构实现最核心的功能。

很多人可能会疑惑，如此极简的设计，真的能应对复杂的编码场景吗？事实上，Pi的极简并不是"简陋"，而是"精准"。它选择的4个内置工具read、write、edit、bash，几乎覆盖了编码Agent的所有核心需求，无论是读取文件、写入内容，还是编辑代码、执行命令，都能通过这4个工具高效完成。而bash工具的引入，更是巧妙地解决了子Agent的需求，通过bash自我调用，Pi可以实现复杂的任务拆分和执行，既保证了功能的完整性，又避免了引入子Agent带来的黑盒问题和观测困难。



与此同时，Pi放弃冗长的系统提示词，也并非降低了对LLM的引导要求，而是充分信任前沿LLM的能力。Mario Zechner认为，与其用大量的token去"教导"LLM如何成为一个Agent，不如用简洁的提示词明确其核心职责，让LLM充分发挥自身的理解和执行能力。这种思路不仅减少了上下文窗口的占用，提升了运行效率，还让Agent的行为更加灵活自主，能够更好地适应不同的场景需求。



二、架构分层解析 5个文件构建的高效运行时
深入探索
函数库
程序设计
应用

Pi的极简主义不仅体现在设计理念上，更贯穿于整个架构实现之中。Pi Agent Core的全部源码只有5个文件，总代码量约1500行，却构建起了一个完整、高效、灵活的Agent运行时。这5个文件分别是types.ts、agent-loop.ts、agent.ts、proxy.ts和index.ts，每个文件对应一个核心模块，各司其职、相互配合，构成了Pi的完整架构。接下来，我们将逐层剖析每个核心模块的设计决策和实现细节，读懂这5个文件背后的精妙设计。



三、类型系统：少即是多 构建灵活可扩展的基础框架


types.ts作为Pi的类型定义文件，是整个架构的基础，它的设计核心是"少即是多"，通过简洁而灵活的类型定义，为后续模块的实现提供了坚实的支撑。其中，最精妙的设计莫过于AgentMessage类型的抽象，它实现了应用状态与模型上下文的分离，这也是Pi能够实现"最晚转换"策略的关键。

深入探索
python
开发语言
分布式计算

在types.ts中，首先定义了一个空接口CustomAgentMessages，这个接口默认是空的，应用层可以通过Declaration Merging的方式对其进行扩展，添加自定义的消息类型。随后，AgentMessage类型被定义为Message类型与CustomAgentMessages所有子类型的联合类型，也就是说AgentMessage既可以是LLM能够直接理解的Message类型，也可以是应用层自定义的各类消息类型。



可能有很多开发者会疑惑，为什么不直接使用LLM的Message类型，反而要额外定义一个AgentMessage类型呢？这其实是Mario Zechner基于真实应用场景的精准考量。在实际的Agent应用中，存在大量"非LLM消息"，这些消息并不需要交给LLM处理，如果直接使用Message类型，就无法对这些消息进行有效区分，要么会导致LLM处理无关消息，影响效率，要么会增加额外的过滤逻辑，增加系统复杂度。



我们可以通过一个具体的场景对比来理解这一设计的优势。在一个编码Agent应用中，用户提问属于UserMessage，Agent回复属于AssistantMessage，工具结果属于ToolResultMessage，这些都是需要交给LLM处理的消息。但与此同时，应用中还会存在UI通知、文件变更事件、会话分支标记等消息，这些消息属于应用层的状态通知，并不需要LLM进行处理，如果交给LLM，不仅会浪费上下文窗口，还可能导致LLM做出错误的判断。



AgentMessage类型的设计，恰好解决了这一问题。应用层通过扩展CustomAgentMessages，可以灵活添加各类自定义消息类型，内部所有逻辑，包括上下文压缩、会话分支管理、UI渲染等，都基于AgentMessage类型运转，只有在调用LLM的瞬间，才会通过convertToLlm方法将AgentMessage数组过滤转换为LLM能够理解的Message数组。这种"最晚转换"策略，不仅保证了应用层的灵活性，还最大限度地减少了LLM的无效处理，提升了系统的运行效率。

除了AgentMessage，types.ts中还定义了AgentLoopConfig接口，这个接口的设计同样体现了Pi的极简主义。AgentLoopConfig继承自SimpleStreamOptions，包含了模型实例、消息转换方法、上下文变换方法、引导消息获取方法、后续消息获取方法以及API Key获取方法等核心配置项。值得注意的是，这个接口中并没有包含maxSteps、maxTokens、temperature等常见的配置项，这些配置项虽然继承自SimpleStreamOptions，但循环本身并不关心这些细节，这种设计让循环逻辑更加纯粹，只专注于核心的任务流转，而将细节配置交给上层模块处理，实现了职责的清晰划分。

另外，types.ts中还定义了AgentEvent类型，通过三层嵌套的生命周期事件，实现了Agent运行过程的细粒度可观测。AgentEvent包含了agent_start、agent_end、turn_start、turn_end、message_start、message_update、message_end以及工具执行相关的各类事件，覆盖了Agent从启动到结束的整个生命周期。这种细粒度的事件设计，让开发者能够清晰地观测到Agent的每一步操作，包括LLM调用、工具执行、消息更新等，彻底解决了主流Agent中存在的黑盒问题，为调试和优化提供了极大的便利。



四、核心循环：双层While设计 实现高效灵活的任务流转
agent-loop.ts是Pi的核心模块，负责实现Agent的核心循环逻辑，也就是Mario Zechner所说的"LLM + tools + a loop"中的loop部分。这个模块的核心设计是双层While循环，外层由FollowUp消息驱动，内层由ToolCall和Steering消息驱动，通过这种精密的循环设计，实现了Agent任务的高效流转和灵活干预，同时保证了系统的简洁性和可维护性。

在深入解析双层循环之前，我们首先要明确Pi的核心循环理念：循环的目的是实现任务的自主执行和灵活调整，不需要复杂的规划模块，只需要通过消息驱动和工具执行，就能完成各类复杂任务。基于这一理念，Pi的核心循环没有设计复杂的规划链路，而是通过简单的循环逻辑，结合消息队列和工具执行，实现了任务的自主推进。

4.1 入口设计：区分场景 实现优雅的重试与恢复
agent-loop.ts中提供了两个核心入口函数，分别是agentLoop和agentLoopContinue，这两个函数的设计体现了Pi对不同场景的精准适配，尤其是实现了优雅的重试与恢复功能。其中，agentLoop函数主要用于用户发送新消息的场景，可以从空上下文开始，启动一轮新的对话和任务执行；而agentLoopContinue函数则主要用于重试或恢复场景，不需要重新构造prompt，直接从当前上下文继续执行任务。

这种入口的区分，看似简单，却解决了Agent开发中的一个常见痛点。在很多主流Agent框架中，当任务执行出错或需要重试时，开发者需要重新构造prompt和上下文，操作繁琐且容易出错。而Pi通过agentLoopContinue函数，让重试变得异常优雅，出错后不需要重新构造任何内容，直接调用该函数，就能从当前上下文继续执行，极大地提升了开发效率和用户体验。





4.2 双层循环：FollowUp与ToolCall双驱动 实现灵活流转
核心循环的核心是双层While结构，这也是Pi能够实现自主任务执行和灵活干预的关键。外层循环由FollowUp消息驱动，主要负责任务的后续推进；内层循环由ToolCall和Steering消息驱动，主要负责工具执行和中途干预，两层循环相互配合，构成了完整的任务流转逻辑。

外层循环的逻辑非常简洁，它是一个无限循环，当内层循环处理完当前的工具调用和消息后，Agent即将停止时，会检查是否有FollowUp消息。如果存在FollowUp消息，就将这些消息设为待处理消息，重启内层循环，继续执行后续任务；如果没有FollowUp消息，就跳出循环，任务正式结束。这种设计的优势在于，能够实现任务的连续推进，用户可以在Agent完成当前任务后，添加后续任务，Agent会自动继续执行，不需要重新启动循环。

内层循环则是任务执行的核心，它同样是一个无限循环，只要存在未处理的工具调用或待处理消息，就会持续执行。内层循环的主要流程包括注入待处理消息、流式调用LLM、执行工具、检查Steering消息等步骤。在这个循环中，Agent会不断地调用LLM生成工具调用指令，执行工具获取结果，再将结果反馈给LLM，生成下一步操作，直到所有工具调用完成，没有待处理消息为止。

值得注意的是，内层循环中加入了Steering中断的实现，这也是Pi实现用户中途干预的关键。当用户在工具执行期间发送Steering消息时，工具执行会立即停止，后续未执行的工具会被标记为"因排队的用户消息而跳过"并返回错误结果，然后跳出内层循环，将Steering消息注入上下文，下一轮LLM调用时，会看到已执行工具的正常结果、被跳过工具的错误结果以及用户的Steering消息，从而理解发生了什么，并做出适当的调整。



这种Steering中断机制，解决了主流Agent中用户无法中途干预的痛点。在很多复杂场景中，用户可能会发现Agent的执行方向出现偏差，此时需要能够及时中断并修正，但大多数主流Agent要么不支持中途干预，要么干预后无法正确处理后续逻辑。而Pi的Steering中断机制，不仅能够实现即时中断，还能通过错误结果标记，让LLM清晰地了解干预情况，做出正确的调整，保证任务能够按照用户的预期推进。

4.3 流式应答：最晚转换 保证上下文实时更新
agent-loop.ts中还实现了streamAssistantResponse函数，这个函数是AgentMessage与Message转换的唯一边界，负责实现流式应答处理，同时保证上下文的实时更新。流式应答是提升用户体验的关键，尤其是在处理复杂任务时，用户能够实时看到Agent的响应过程，而不需要等待整个任务完成。

streamAssistantResponse函数的执行流程可以分为五个步骤。第一步是可选的上下文变换，通过config.transformContext方法，可以对上下文消息进行剪枝、注入外部上下文等操作，从而优化上下文质量，减少无效信息的占用。第二步是消息转换，通过config.convertToLlm方法，将AgentMessage数组转换为LLM能够理解的Message数组，这也是"最晚转换"策略的核心实现点，只有在调用LLM的瞬间才进行转换，最大限度地保证了应用层的灵活性。

第三步是动态解析API Key，支持过期OAuth Token的动态更新，通过config.getApiKey方法，能够根据模型提供商的不同，动态获取对应的API Key，避免了API Key硬编码带来的安全隐患和维护问题。第四步是流式调用LLM，通过streamFunction方法，调用模型的流式接口，获取实时的响应事件。第五步是实时转发事件，将LLM返回的每个delta事件实时转发给UI，同时将部分消息就地更新到上下文消息数组中，确保上下文始终是当前最新状态。



这里有一个非常关键的细节，部分消息会被就地更新到context.messages数组中，也就是说，上下文消息数组会随着LLM的流式响应实时更新，而不是等到整个响应完成后再更新。这种设计的优势在于，无论任务执行到哪一步，上下文都能反映当前的最新状态，当出现重试、干预等场景时，LLM能够基于最新的上下文做出判断，避免了因上下文滞后导致的错误。

五、Agent类：状态容器+消息队列 实现灵活的任务管理
agent.ts模块实现了Agent类，这个类的核心作用是作为状态容器和消息队列，负责管理Agent的运行状态、处理消息队列、实现错误恢复等功能。Agent类的设计同样遵循极简主义理念，没有复杂的逻辑，却能实现灵活高效的任务管理，为上层应用提供了简洁易用的接口。

5.1 队列模式：两种模式适配不同场景需求
Agent类引入了两种队列模式，分别是Steering模式和FollowUp模式，每种模式都支持"all"和"one-at-a-time"两种类型。其中，Steering模式用于处理用户中途发送的干预消息，FollowUp模式用于处理用户添加的后续任务消息，两种模式相互独立，确保了消息处理的有序性和灵活性。

"all"模式表示一次性发送所有队列中的消息，适用于消息数量较少、不需要逐一条理的场景；"one-at-a-time"模式表示每次只发送一条消息，处理完成后再发送下一条，适用于消息数量较多、需要逐条处理的场景。为什么需要"one-at-a-time"模式呢？我们可以通过一个具体的场景来理解。假设用户在Agent工作时快速发送了3条修正消息，如果使用"all"模式，Agent会一次性收到这3条消息，很可能只会回应最后一条，导致前两条修正消息被忽略；而使用"one-at-a-time"模式，Agent会逐条处理这3条消息，每条消息都能得到充分的响应，确保用户的所有需求都能被满足。

编程



这种队列模式的设计，充分考虑了真实应用中的场景需求，让Agent能够根据不同的消息类型和数量，灵活选择合适的处理方式，既保证了处理效率，又避免了消息遗漏或处理不充分的问题。

5.2 核心方法：清晰划分职责 实现灵活调用
Agent类提供了一系列核心方法，包括prompt、continue、steer、followUp、abort等，每个方法都有明确的调用时机和功能，职责划分清晰，方便上层应用灵活调用。

prompt方法用于Agent空闲时，用户发送新消息的场景，调用该方法会启动一轮新的对话和任务执行，将用户消息添加到待处理队列中，触发核心循环。continue方法用于Agent空闲时，需要重试或恢复任务的场景，调用该方法会从当前上下文继续执行任务，不需要重新构造prompt和上下文。steer方法用于Agent工作时，用户需要中途干预的场景，调用该方法会中断当前的工具链，将干预消息添加到Steering队列中，触发内层循环的重新执行。

followUp方法用于任何时候，用户需要添加后续任务的场景，调用该方法会将后续任务消息添加到FollowUp队列中，当当前任务完成后，Agent会自动执行后续任务。abort方法用于Agent工作时，需要取消当前任务的场景，调用该方法会取消当前的LLM调用，终止任务执行，并标记任务状态为"aborted"。

这些方法的设计，让上层应用能够根据不同的场景需求，灵活调用对应的方法，实现对Agent的精准控制。同时，方法的命名简洁易懂，降低了上层应用的开发难度，提升了开发效率。

5.3 错误恢复：优雅处理异常 保证系统稳定性
错误恢复是Agent开发中的一个重要环节，尤其是在复杂的任务场景中，很容易出现LLM调用失败、工具执行错误等问题，如果不能妥善处理这些错误，很可能导致系统崩溃或任务无法继续执行。Pi的Agent类在错误处理方面做了非常细致的设计，能够优雅地处理各类异常，保证系统的稳定性。

软件



在Agent类的核心逻辑中，所有可能出现错误的地方都添加了try-catch捕获机制，当出现错误时，会在catch块中构造一个完整的AssistantMessage，该消息包含了错误状态、错误信息等内容，其中stopReason被标记为"error"，如果是用户主动取消，则标记为"aborted"。然后，将这个错误消息添加到上下文消息数组中，确保错误状态能够成为上下文历史的一部分。

这种设计的优势在于，当任务执行出现错误时，用户可以通过调用continue方法，从错误处重试，LLM会看到之前的错误信息，并据此调整策略，避免再次出现相同的错误。同时，错误消息被添加到上下文后，也方便开发者调试和分析错误原因，提升系统的可维护性。

六、Proxy Stream：带宽优化 适配Web应用场景
proxy.ts模块是Pi为Web应用场景设计的核心模块，主要用于解决Web应用中LLM调用的带宽优化问题。在Web应用场景中，Agent的运行流程通常是浏览器发送请求到代理服务器，代理服务器调用LLM接口，再将响应结果返回给浏览器。如果直接传输完整的响应消息，尤其是流式响应中的部分消息，会占用大量的带宽，影响响应速度，尤其是在网络环境较差的情况下，这种影响会更加明显。

为了解决这一问题，Pi的Proxy Stream模块引入了一种带宽优化策略，服务器不传输完整的部分消息对象，仅传输轻量的delta事件。具体来说，ProxyAssistantMessageEvent比原始的AssistantMessageEvent轻得多，它去掉了完整的部分字段，仅包含contentIndex、delta等最小必要信息。客户端通过processProxyEvent方法，根据这些delta事件，逐步重建完整的响应消息，从而减少了带宽占用，提升了响应速度。

这种设计的核心思路是"按需传输"，只传输客户端重建完整消息所必需的最小信息，避免传输冗余数据。通过这种方式，不仅能够节省带宽，还能提升响应速度，改善用户体验，让Pi能够更好地适配Web应用场景的需求。同时，Proxy Stream模块的设计也保持了极简主义，没有引入复杂的压缩算法，仅通过数据裁剪的方式实现带宽优化，兼顾了效率和简洁性。

操作系统



其他内容介绍：
插件可扩展性
那么OpenClaw 怎么让别人在不改核心代码的情况下，扩展新功能？OpenClaw 的设计哲学是开放扩展，不改核心。

可以通过插件（Plugin）在四个方向上扩展：

渠道插件（Channel Plugin）：添加新的聊天平台，比如 飞书、钉钉等
记忆插件（Memory Plugin）：换一种存储后端，比如用向量数据库代替默认的 SQLite
工具插件（Tool Plugin）：添加自定义能力，比如除了内置的命令行、浏览器、文件操作之外的新工具
模型提供商插件（Provider Plugin）：接入自定义的 AI 模型或自己部署的模型
插件代码放在 extensions/ 目录下，系统会自动扫描、发现和加载。




沙箱的运行
OpenClaw可以使用基于Docker的沙箱技术，用于在Agent执行工具时提供运行时隔离与权限约束，以降低对宿主环境的影响范围，并形成可配置的最小权限执行面。其使用场景主要体现在以下功能层面：

会话级隔离：在多会话或群聊场景中，非主会话可配置为在隔离环境中执行工具调用，使其与宿主进程、宿主文件系统、主会话的执行上下文分离，减少非主入口对宿主资源的直接访问。
文件系统访问控制：沙箱可限制工具读写范围到特定工作区，并支持不同的访问级别（如不挂载宿主工作区、只读挂载或读写挂载）。工具对文件路径的解析与访问会被约束在沙箱根目录内，避免越界读写。
命令执行隔离：当启用沙箱模式时，命令执行工具在隔离环境中运行，执行环境的工作目录、环境变量与资源限制由沙箱配置控制，防止命令直接在宿主环境中运行。
工具可用性与策略约束：沙箱支持工具级 allow/deny 策略。即使工具在全局可用，进入沙箱后仍会按沙箱策略过滤，确保在隔离会话中只暴露经过许可的工具能力。
浏览器执行隔离：沙箱可为浏览器能力提供独立的隔离运行环境，并控制是否允许宿主级浏览器控制或仅允许沙箱内控制，以限制网页自动化对宿主浏览器环境的影响。
会话工具可见性限制：当会话处于沙箱模式时，可限制会话管理类工具对其他会话的可见性与操作范围，例如仅允许访问由当前会话派生的子会话，避免跨会话访问与干预。
运维与诊断：沙箱提供容器级管理与解释能力，支持查看沙箱容器状态、重建容器以及解释当前会话的生效策略，用于排查策略阻断或运行异常问题。
总结
OpenClaw将自己的仓库直接交给DeepWiki自动生成Doc，并支持通过Devin Agent来询问关于仓库的任何信息。

不过以OpenClaw的Doc的质量来看，很难说这是因为OpenClaw的代码质量不够好，还是DeepWiki的生成质量不好，总之这份Doc相当的糟糕，大量的重复与冗余，不仅没有将代码的工作流程梳理清楚，有些地方似乎还形成误导。而尝试通过Devin来滤清代码的脉络，其效果又远不如Codex。

说回到项目本身，涉及到Agent的部分几乎没有突破式的创新，但是如何将不同IM渠道，不同设备整合在一单一的WebSocket Server之上，还是能学到很多工程方面的知识。


一文读懂：openClaw 分析与教程+免费大模型（Moltbot、Clawdbot） 二更
863 赞同 · 78 评论 文章

一文揭秘: OpenClaw的底层技术与核心功能Moltbot/Clawdbot/
26 赞同 · 4 评论 文章
PI agents实现代码说明：
pi-ai：LLM 抽象层
pi-ai 中有最有趣的工程实践。Mario 识别出每个供应商都说四种通信协议之一：

OpenAI Completions (/v1/chat/completions)
OpenAI Responses (/v1/responses — 较新的结构化格式)
Anthropic Messages (/v1/messages)
Google Generative AI (/v1beta/models/{model}:generateContent)
Pi 围绕这四种协议进行规范化，并维护一个300+ 模型定义的目录，在构建时从 models.dev 和 OpenRouter 元数据自动生成：

interface ModelDefinition {
  id: string;
  provider: string;
  protocol: 'openai-completions' | 'openai-responses' | 'anthropic' | 'google';
  contextWindow: number;
  maxOutputTokens: number;
  inputCostPer1M: number;
  outputCostPer1M: number;
  capabilities: {
    vision: boolean;
    toolUse: boolean;
    streaming: boolean;
    reasoning: boolean;
  };
}
抽象层内部处理数十个供应商特定的特性。结果：调用 300+ 模型中的任何一个看起来都一样：

import { getModel, stream, Context } from '@mariozechner/pi-ai';


const model = getModel('anthropic', 'claude-sonnet-4-20250514');


const context: Context = {
  systemPrompt: 'You are a helpful assistant.',
  messages: [{ role: 'user', content: 'Hello' }]
};


for await (const event of stream(model, context)) {
  if (event.type === 'text_delta') {
    process.stdout.write(event.delta);
  }
}
工具使用 TypeBox schema 和 AJV 验证。当验证失败时，错误作为工具结果返回给模型，让它可以自我纠正：

import { Type } from '@mariozechner/pi-ai';


const tools = [{
  name: 'read_file',
  description: 'Read contents of a file',
  parameters: Type.Object({
    path: Type.String({ description: 'Absolute path to file' }),
    startLine: Type.Optional(Type.Number({ description: 'Start line (1-indexed)' })),
    endLine: Type.Optional(Type.Number({ description: 'End line (1-indexed)' }))
  })
}];
这消除了整类运行时故障——模型在同一轮内从错误中学习。

上下文可移植性
上下文对象完全可序列化，可跨供应商移植：

interface Context {
  systemPrompt: string;
  messages: Message[];
  tools?: Tool[];
  temperature?: number;
  maxTokens?: number;
}

interface Message {
  role: 'user' | 'assistant';
  content: string | ContentBlock[];
  toolCalls?: ToolCall[];
  toolResults?: ToolResult[];
  thinking?: string; 
}






Agent 循环：极致简单
pi-agent-core 中的核心循环故意极简：

async function* agentLoop(
  model: Model,
  context: Context,
  tools: Tool[],
  options: AgentOptions
): AsyncGenerator<AgentEvent> {
  while (true) {
    // 1. 调用 LLM
    const response = await streamCompletion(model, { ...context, tools });

    // 流式发送助手消息
    yield { type: 'assistant_message', content: response.content };

    // 2. 没有工具调用则完成
    if (!response.toolCalls?.length) {
      break;
    }

    // 3. 执行工具调用
    const results: ToolResult[] = [];
    for (const call of response.toolCalls) {
      yield { type: 'tool_start', name: call.name, input: call.input };

      // 执行并捕获结果
      const result = await executeToolCall(call, context);
      results.push(result);

      yield { type: 'tool_end', name: call.name, result };
    }

    // 4. 将助手消息 + 工具结果添加到上下文
    context.messages.push({
      role: 'assistant',
      content: response.content,
      toolCalls: response.toolCalls
    });
    context.messages.push({
      role: 'user',
      toolResults: results
    });
  }
}
就是这样。没有最大步数设置（"循环一直运行直到 Agent 说它完成了"），没有计划模式（"把计划写入文件——完全可观察"），没有内置待办跟踪（"待办列表通常让模型更困惑而不是帮助"）。

工具结果结构
每个工具结果都包含 output（给 LLM 的文本）和 details（给 UI 渲染的结构化数据）：

interface ToolResult {
  toolCallId: string;
  output: string;        // 发送给 LLM 的文本
  details?: {            // UI 渲染的结构化数据
    type: 'file' | 'diff' | 'terminal' | 'error';
    data: unknown;
  };
}

// 示例
{
  output: "Contents of /src/app.ts:\n```typescript\nexport function main() {...}\n```",
  details: {
    type: 'file',
    data: {
      path: '/src/app.ts',
      content: 'export function main() {...}',
      language: 'typescript',
      lineCount: 42
    }
  }
}
这种分离保持模型上下文干净，同时启用丰富的 UI 渲染。





四个内置工具
Pi 只提供四个工具：

1. read — 文件和图片读取
const readTool = {
  name: 'read',
  description: 'Read file contents or view images',
  parameters: Type.Object({
    paths: Type.Array(Type.String(), { 
      description: 'Absolute paths to files or images' 
    }),
    startLine: Type.Optional(Type.Number()),
    endLine: Type.Optional(Type.Number())
  })
};
2. write — 文件创建
const writeTool = {
  name: 'write',
  description: 'Create or overwrite a file',
  parameters: Type.Object({
    path: Type.String({ description: 'Absolute path' }),
    content: Type.String({ description: 'File content' })
  })
};
3. edit — 精确文本替换
const editTool = {
  name: 'edit',
  description: 'Replace text in a file using search/replace',
  parameters: Type.Object({
    path: Type.String(),
    edits: Type.Array(Type.Object({
      search: Type.String({ description: 'Exact text to find' }),
      replace: Type.String({ description: 'Replacement text' })
    }))
  })
};
edit 工具需要精确字符串匹配——没有正则，没有模糊匹配。如果搜索字符串不精确匹配一次，它会失败并给出清晰的错误。这迫使模型先读取文件并使用精确字符串。

4. bash — 命令执行
const bashTool = {
  name: 'bash',
  description: 'Execute a bash command',
  parameters: Type.Object({
    command: Type.String({ description: 'Command to execute' }),
    timeout: Type.Optional(Type.Number({ default: 30000 }))
  })
};
没有后台进程——如果需要，使用 tmux。工具同步运行命令并返回完整输出。

为什么没有更多工具？
Mario 的论点："所有前沿模型都经过了大量 RL 训练，所以它们天生理解什么是编程 Agent。" 模型知道 bash 是什么。它知道文件如何工作。添加专门的工具（如"在代码库中搜索"工具）只是给系统提示词添加 token 而不增加能力。

如果你需要 ripgrep，通过 bash 运行 rg。如果你需要搜索 GitHub，通过 bash 使用 gh。模型可以阅读文档并弄清楚 CLI 工具。





系统提示词：不到 1000 token
比较 Claude Code 数千 token 的系统提示词与 Pi 的：

You are Pi, a coding assistant. You help users write, debug, and understand code.

You have access to these tools:
- read: Read files and images
- write: Create/overwrite files  
- edit: Make surgical edits to files
- bash: Run shell commands

Work directly in the user's project. Read files to understand context before making changes. Use bash to run tests, linters, and other tools.

Think step by step. If you're unsure, read more files or ask the user.
这大约是整个系统提示词。其他一切——项目上下文、约定、文档——都来自磁盘上的文件。

AGENTS.md：项目特定上下文
Pi 从项目根目录和子目录读取 AGENTS.md 文件：

# AGENTS.md

## Project Overview
This is a React + TypeScript monorepo using pnpm workspaces.

## Commands
- `pnpm dev` - Start development server
- `pnpm test` - Run tests
- `pnpm lint` - Run linting

## Conventions
- Use functional components with hooks
- Put tests in `__tests__` folders
- Use kebab-case for file names
Agent 在进入目录时读取这些，仅在相关时获取上下文。





会话格式：仅追加 DAG
会话持久化为 .jsonl 文件，具有特定结构：

interface SessionEntry {
  id: string;          // 唯一 ID
  parentId?: string;   // 父条目 ID
  type: 'message' | 'tool_call' | 'tool_result' | 'meta';
  timestamp: number;
  data: unknown;
}
格式是仅追加 DAG（有向无环图）。每个条目有一个 ID 和可选的父 ID，形成树结构。分支只是移动"叶指针"而不是创建新文件：

    ┌── entry1 ── entry2 ── entry3 (main branch)
    │                    └── entry4 (branched)
    └── entry5 ── entry6 (another branch)
切换分支只是改变你追加到哪个叶子。整个历史都被保留。

跨供应商会话
会话可以包含来自多个供应商的消息：

{"id":"1","type":"message","data":{"role":"user","content":"Explain this code"}}
{"id":"2","parentId":"1","type":"message","data":{"role":"assistant","content":"...","model":"anthropic/claude-sonnet-4-20250514"}}
{"id":"3","parentId":"2","type":"message","data":{"role":"user","content":"Now using GPT-4"}}
{"id":"4","parentId":"3","type":"message","data":{"role":"assistant","content":"...","model":"openai/gpt-4o"}}
Thinking 跟踪在跨越供应商时被规范化为文本格式。