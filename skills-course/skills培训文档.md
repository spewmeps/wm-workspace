# Skills 开发与应用指南

本培训文档旨在帮助开发者和运维人员首先深入理解 Skills 的核心机制与开发范式，进而掌握基于 Witty 平台开发和执行 Skills 的流程。Witty 平台主要包含两个核心项目：[witty-diagnosis-agent](https://atomgit.com/openeuler/witty-diagnosis-agent) 专注于工程化在运维场景的执行和分析流程，通过标准化故障诊断步骤和自动化分析工具，显著提升故障诊断效率；[witty-skill-insight](https://atomgit.com/openeuler/witty-skill-insight) 围绕 Skills 的生命周期管理（包括 Skill 生成、链路跟踪、迭代优化），通过智能化工具降低 Skills 的使用门槛，让开发者和运维人员能够更高效地创建、应用和优化 Skills。

**核心目标**：
1.  **Skills 核心概念**：阐述 Skills 的基本能力与优势，解析 Witty 框架与 Skills 的关系。
2.  **开发入门**：详解 Skills 基础规范与编写指南，介绍自动化生成 Skills 的方法。
3.  **进阶实践**：探讨如何生成高质量 Skills，以及持续迭代优化的策略。
4.  **运维适配**：分享运维场景下生成与应用 Skills 的最佳实践。
5.  **全生命周期管理**：基于 Witty 框架，解析 Skills 的全生命周期管理（生成、执行、优化、链路跟踪）。

---

**目录**

*   [1. Skills 核心概念](#1-skills-核心概念)
*   [2. 开发入门：基础与自动化生成](#2-开发入门基础与自动化生成)
*   [3. 进阶实践：高质量 Skills 与迭代优化](#3-进阶实践高质量-skills-与迭代优化)
*   [4. 运维适配：场景化实践](#4-运维适配场景化实践)
*   [5. 基于 Witty 框架的 Skills 全生命周期](#5-基于-witty-框架的-skills-全生命周期)
*   [6. 参考资料](#6-参考资料)

---


## 1. Skills 核心概念
> **本章目标**：阐述 Skills 的基本能力与优势，以及其作为业界通用标准的价值。

*   **1.1 Skills 的基本组成**
    *   **定义**：Skills 是一种模块化的能力扩展系统，为 AI 提供了特定领域的专业知识、工作流程和最佳实践。
    *   **技能结构 (Structure of a skill)**：
        每个技能都必须遵循此目录结构：
        ```text
        skill-name/
        ├── SKILL.md              # Required: Metadata + core instructions (<500 lines)
        ├── scripts/              # Executable code (Python/Bash) designed as tiny CLIs
        ├── references/           # Supplementary context (schemas, cheatsheets)
        └── assets/               # Templates or static files used in output
        ```
        *   **SKILL.md**: 作为"大脑"使用,用于导航和高层次流程。
        *   **References**: 参考文献：直接从 SKILL.md 链接。仅保留一级深度。
        *   **Scripts**: 脚本：用于脆弱/重复性操作，其中变化是错误。不要在此处捆绑库代码。
    *   **组成要素**：
        *   📘 **元数据 (Metadata)**：始终加载的轻量级描述，告诉 AI 有哪些能力可用（`SKILL.md` Frontmatter）。
        *   📄 **指令 (Instructions)**：触发时加载的操作指南，提供具体的 Quick Start 和流程（`SKILL.md` 内容）。
        *   🛠️ **资源与代码 (Resources & Code)**：按需访问的详细参考文档（`REFERENCE.md`）和工具脚本（`scripts/*.py`）。
    *   **核心特点**：
        *   📦 **模块化封装**：将指令、脚本、资源文件组织成独立的技能包。
        *   🔄 **按需加载**：采用**渐进式披露 (Progressive Disclosure)** 机制，仅在需要时加载相关上下文，节省 Token。
        *   🔌 **可组合性**：多个 Skills 可以协同工作，构建复杂工作流（如：数据采集 -> 分析 -> 报告）。
        *   ♻️ **可复用性**：一次创建，跨多个对话和项目自动复用。
*   **1.2 Skills 的运行原理：渐进式披露**
    
    Skills 采用三级加载机制，确保高效利用上下文窗口：

    | 层级 | 加载时机 | Token 消耗 | 作用 |
    | :--- | :--- | :--- | :--- |
    | **Level 1：元数据** (感知层) | 系统启动时 | 极低 (~100 tokens) | 让 AI 知道“我会什么”，建立能力索引 |
    | **Level 2：指令** (决策层) | 匹配 Skill 时 | 中等 (< 5k tokens) | 加载具体步骤，指导 AI “怎么做” |
    | **Level 3：资源** (执行层) | 具体步骤引用时 | 按需 (脚本无消耗) | 提供精准 API 参考或执行具体脚本 |

*   **1.3 为什么 Skills 执行效果更好？**
    *   💾 **零成本存储**：未使用的知识文件静置在磁盘中，不占用宝贵的 Context Window。
    *   🎯 **专注与准确**：通过加载特定领域的上下文，减少通用模型的幻觉，使其成为“领域专家”。
    *   ✅ **标准化流程**：将最佳实践固化为代码和文档，确保每次执行的一致性，避免遗漏步骤。
*   **1.4 业界标准与 Witty 的关系**
    *   🌐 **通用标准**：Skills 遵循业界通用的 Agent Skills 规范（如 Anthropic `agentskills`），具有高度的可移植性。
    *   🚀 **跨平台支持**：标准的 Skill 可以在 Claude Code, OpenCode, OpenClaw 等任何支持该规范的 Agent 平台上运行。
    *   🧠 **Witty, OpenCode/OpenClaw 与 Skills 的三角关系**：
        *   **OpenCode/OpenClaw (底座)**：基于 Bun/TypeScript 的高性能Agent运行时，负责插件加载、沙箱执行与资源调度，是“神经系统”。
        *   **Skills (能力)**：运行在 OpenCode/OpenClaw 上的标准化插件，封装了具体业务逻辑，是“手脚”。
        *   **Witty (应用)**：基于 OpenCode/OpenClaw 构建的上层智能运维应用，负责意图决策与全链路编排，是“大脑”。
*   **1.5 Skills 与相关概念的辨析**
    | 概念 | 定义 | 与 Skills 的区别与关联 |
    | :--- | :--- | :--- |
    | **Skills** | **模块化能力包** | 包含指令、代码与资源，是 Agent 的“技能插件”。 |
    | **Prompt (提示词)** | **通用指令/人设** | Prompt (如 System Prompt) 通常全局生效且常驻上下文；Skills 是**按需加载**的**垂直能力包**，包含代码与专业知识，不占用无关对话的 Token。 |
    | **MCP (Model Context Protocol)** | **连接协议** | MCP是连接外部服务的电话（Gmail、Drive、Slack等API）, Skills 指导完成任务的教科书（如何创建Word、PPT、架构设计等最佳实践） |
    | **Agent (智能体)** | **自主决策实体** | Agent 是 Skills 的使用者与调度者。没有 Skills，Agent 只有大脑没有手脚；没有 Agent，Skills 只是静态的模块。 |

## 2. 开发入门：基础与自动化生成
> **本章目标**：详解 Skills 基础规范与编写指南，介绍自动化生成 Skills 的方法。

*   **2.1 基础规范清单 (Checklist)**

    | 规范维度 | 要求说明 | 示例/备注 |
    | :--- | :--- | :--- |
    | **目录命名** | 必须使用 kebab-case（小写，短横线），禁止空格、下划线或大写 | ✅ `notion-project-setup` <br> ❌ `Notion Project Setup` |
    | **核心文件** | 必须包含 `SKILL.md`（大小写敏感），禁止变体 | ✅ `SKILL.md` <br> ❌ `skill.md`, `SKILL.MD` |
    | **元数据** | 必须包含 YAML Frontmatter，且包含 `name` 和 `description` | `name` 必须与目录名匹配且为 kebab-case |
    | **描述字段** | `description` 必须包含**功能**（做什么）和**触发时机**（何时用） | "Analyzes logs. Use when user asks to debug..." |
    | **安全限制** | Frontmatter 中禁止使用 XML 尖括号 `< >` | 防止 Prompt 注入攻击 |
    | **文档规范** | 禁止在 Skill 目录内包含 `README.md` | 所有文档应在 `SKILL.md` 或 `references/` 中 |
    | **脚本规范** | 无状态 CLI 工具，通过 `stdout` 输出结果 | 推荐 Python3 (标准库) 或 Bash |

*   **2.2 自动化生成 (The Easy Way)**
    > **强烈建议**：始终使用Skill升成工具来生成 Skill，**不要手动从零开始编写**。这能确保结构正确、避免语法错误，并大大节省时间。
    *   **Skill Creator 工具**：
        *   **简介**：`skill-creator` 是一个“制造 Skill 的 Skill”（Meta-Skill），它能将自然语言需求转化为标准化的 Skill 结构。
        *   **工作流**：
            1.  **Intent**：告诉 AI "我想做一个查询 K8s Pod 日志的 Skill"。
            2.  **Draft**：工具自动生成 `SKILL.md` 和初步脚本。
            3.  **Refine**：通过多轮对话优化指令和代码。
            4.  **Test**：自动生成测试用例并验证。
    *   **从文档转化**：
        *   将现有的运维手册（Runbook）、API 文档或 Wiki 页面直接喂给 Skill Creator，快速提取关键步骤和参数，转化为可执行的 Skill。
        *   💡 **运维场景优先建议**：这是运维场景下生产 Skill 的最高效路径。不要从零手写，而是先将现有的 SOP（标准作业程序）文档化，然后让 AI 转化为 Skill。这确保了 Skill 的执行逻辑与经过验证的人工流程完全一致。

## 3. 进阶实践：高质量 Skills 与迭代优化
> **本章目标**：探讨如何生成高质量 Skills，以及持续迭代优化的策略。

*   **3.1 规划与设计：从用例开始 (Planning & Design)**
    *   **原则**：在编写任何代码之前，先明确你的 Skill 应支持的 2–3 个具体用例。
    *   **良好的用例定义示例**：
        *   **用例 (Use Case)**：项目冲刺规划 (Sprint Planning)
        *   **触发条件 (Trigger)**：用户说“帮我规划这个冲刺”或“创建冲刺任务”
        *   **步骤 (Steps)**：
            1.  从 Linear 获取当前项目状态（通过 MCP）。
            2.  分析团队速度（velocity）和产能。
            3.  建议任务优先级排序。
            4.  在 Linear 中创建任务，并设置合适的标签和工时估算。
        *   **结果 (Result)**：完成一个完整规划的冲刺，并已创建好所有任务。
    *   **自问清单 (Ask yourself)**：
        *   用户想要完成什么？(What)
        *   这需要哪些多步骤的工作流程？(Workflow)
        *   需要用到哪些工具（内置工具或 MCP）？(Tools)
        *   应嵌入哪些领域知识或最佳实践？(Domain Knowledge)
    *   **定义成功标准 (Define Success Criteria)**：
        *   **定量指标 (Quantitative Metrics)**：
            *   **触发率 > 90%**：在 10-20 条相关查询中，Skill 应自动加载，无需显式调用。
            *   **效率提升**：对比无 Skill 场景，完成相同任务的工具调用次数和 Token 消耗显著减少。
            *   **API 零失败**：工作流执行期间，MCP 服务端无重试或错误日志。
        *   **定性指标 (Qualitative Metrics)**：
            *   **零人工干预**：用户无需提示“下一步做什么”，也无需修正输出结果。
            *   **结果一致性**：对同一请求运行 3-5 次，输出结构和质量保持一致。
            *   **低认知门槛**：新用户在最少指导下即可首次尝试成功。

*   **3.2 测试与迭代 (Testing and Iteration)**
    *   **测试层级**：
        1.  **触发测试 (Triggering Tests)**: 确保 Skill 在正确的时候加载（"Help me set up project" -> ✅, "What's the weather" -> ❌）。
        2.  **功能测试 (Functional Tests)**: 验证输出正确，API 调用成功。
        3.  **性能对比 (Performance Comparison)**: 对比使用 Skill 前后的 Token 消耗和交互轮数。
    *   **迭代策略：基于反馈的优化 (Iteration based on feedback)**：
        *   Skill 是“活”的文档，需根据实际信号持续迭代。
        *   **触发不足 (Undertriggering)**：
            *   *信号*：Skill 未按预期加载、用户需手动启用、频繁询问“怎么用”。
            *   *方案*：丰富 `description` 细节，增加具体的关键词和技术术语。
        *   **过度触发 (Overtriggering)**：
            *   *信号*：无关查询也加载 Skill、用户手动禁用、混淆了 Skill 的用途。
            *   *方案*：添加负面触发词（Negative Triggers，如 "Do NOT use for..."），缩小适用范围，更具体化。
        *   **执行问题 (Execution Issues)**：
            *   *信号*：结果不一致、API 调用失败、用户需人工纠正。
            *   *方案*：优化指令清晰度，增加错误处理逻辑（Error Handling），将复杂逻辑固化为脚本。

*   **3.3 设计模式 (Design Patterns)**
    *   **概述**：以下模式源自实践验证。选择模式时，可基于“问题优先”或“工具优先”的思路。

    | 模式名称 | 特点 | 适用场景 |
    | :--- | :--- | :--- |
    | **1. 顺序工作流编排** | 严格的步骤顺序，具有依赖关系 | 用户注册、环境初始化、发布流程 |
    | **2. 多 MCP 协同** | 跨越多个服务/工具的数据流转 | 设计稿交付（Figma->Drive->Linear->Slack） |
    | **3. 迭代优化** | 循环改进，直到满足质量标准 | 报告生成、代码编写、翻译 |
    | **4. 上下文感知选择** | 动态决策，根据条件分流 | 文件存储（大文件->S3, 代码->Git）、智能路由 |
    | **5. 领域知识嵌入** | 在执行前强制植入合规与最佳实践 | 金融支付（风控->执行->审计）、生产变更 |

    *   **Pattern 1: 顺序工作流编排 (Sequential Workflow Orchestration)**
        ```markdown
        # 工作流：新客户入驻
        ### 步骤 1: 创建账户
        调用 MCP 工具: `create_customer` (参数: 姓名, 邮箱)
        ### 步骤 2: 设置支付
        调用 MCP 工具: `setup_payment_method`
        等待: 支付验证完成
        ### 步骤 3: 创建订阅
        调用 MCP 工具: `create_subscription` (参数: 计划ID, 客户ID)
        ### 步骤 4: 发送欢迎邮件
        调用 MCP 工具: `send_email` (模板: welcome_template)
        ```

    *   **Pattern 2: 多 MCP 协同 (Multi-MCP Coordination)**
        ```markdown
        # 工作流：设计到开发交付
        ### 阶段 1: 设计导出 (Figma MCP)
        1. 从 Figma 导出设计资产
        2. 生成设计规范
        ### 阶段 2: 资产存储 (Drive MCP)
        1. 在 Drive 中创建项目文件夹
        2. 上传所有资产 & 生成共享链接
        ### 阶段 3: 任务创建 (Linear MCP)
        1. 创建开发任务
        2. 将资产链接附加到任务 & 分配给团队
        ### 阶段 4: 通知 (Slack MCP)
        1. 发布交付摘要到 #engineering 频道
        ```

    *   **Pattern 3: 迭代优化 (Iterative Refinement)**
        ```markdown
        # 工作流：迭代式报告生成
        ### 初稿
        1. 通过 MCP 获取数据 & 生成初稿
        ### 质量检查
        1. 运行验证脚本: `scripts/check_report.py`
        2. 识别问题: 缺失章节, 格式错误
        ### 优化循环
        1. 解决每个识别出的问题
        2. 重新生成受影响的章节 & 重新验证
        3. 重复直到满足质量阈值
        ### 定稿
        1. 应用最终格式 & 保存最终版本
        ```

    *   **Pattern 4: 上下文感知工具选择 (Context-aware Tool Selection)**
        ```markdown
        # 工作流：智能文件存储
        ### 决策树
        1. 检查文件类型和大小
        2. 确定最佳存储位置:
           - 大文件 (>10MB): 使用云存储 MCP
           - 协作文档: 使用 Notion/Docs MCP
           - 代码文件: 使用 GitHub MCP
        ### 执行存储
        基于决策: 调用相应的 MCP 工具 & 生成链接
        ### 提供上下文
        向用户解释选择该存储方式的原因
        ```

    *   **Pattern 5: 领域知识嵌入 (Domain-specific Intelligence)**
        ```markdown
        # 工作流：带合规检查的支付处理
        ### 处理前 (合规检查)
        1. 获取交易详情
        2. 应用合规规则: 制裁名单, 司法管辖区, 风险等级
        ### 处理
        如果 (IF) 合规通过:
           - 调用支付处理 MCP 工具
        否则 (ELSE):
           - 标记为待审查 & 创建合规工单
        ### 审计追踪
        - 记录所有检查 & 决策
        ```

    *   **常见问题排查 (Troubleshooting)**
        *   **Skill 无法上传**：检查文件名是否严格为 `SKILL.md`（大小写敏感），YAML Frontmatter 格式是否正确。
        *   **Skill 不触发**：检查 `description` 是否包含具体的触发短语（Trigger Phrases），避免过于宽泛。
        *   **Skill 触发太频繁**：添加负面触发词（"Do NOT use for..."），明确适用范围。
        *   **MCP 调用失败**：验证 MCP Server 连接状态、API Key 权限、Tool 名称拼写。
        *   **指令未遵循**：简化指令（Bullet points），将关键指令置顶（## CRITICAL），消除歧义，或将复杂验证逻辑移至脚本中。
        *   **上下文过大/变慢**：优化 `SKILL.md` 大小（<5000 词），将详细文档移至 `references/`，减少同时启用的 Skill 数量。



## 4. 运维适配：场景化实践
> **本章目标**：分析运维场景的独特挑战，分享生成与应用 Skills 的最佳实践。

*   **4.1 运维场景的典型特点 (Typical Characteristics)**
    *   **问题及诊断的复杂性 (Complexity)**
        *   **问题描述**：故障场景多样（实时故障/历史故障、在线分析/离线分析），依赖工具不一（通用/独有），且问题描述存在差异。
        *   **解决方案**：定义标准的故障信息收集流程，在问题诊断前，收集：故障描述，故障时间，故障环境信息，故障诊断场景等，越是标准和详细的信息，有助于故障高效诊断。
        *   <TODO>
    *   **时间维度的重要性**
        *   **问题描述**：故障发生的时间窗口（Time Window）是过滤日志和指标的关键变量，锁定正确的时间窗能让排查事半功倍。
        *   **解决方案**：要求用户提供时间维度，可以是模糊的时间段，尽量减少搜索范围。
        *   <TODO>
    *   **干扰项与根因分析**
        *   **问题描述**：单一故障往往引发连锁反应（级联报错），Skill 需要具备从海量“症状”中识别“病因”的逻辑判断能力，而非仅匹配报错关键词。
        *   **解决方案**：1. 从工程层面使用"假设-推断"的标准分析方法，进行多个维度的问题分析；2. skills里面添加反思的方法论，让模型深入分析。
        *   <TODO>
    *   **高风险性 (High Risk)**
        *   **问题描述**：运维操作（尤其是诊断和修复）通常需要 **Root 权限**，任何误操作（如 `rm -rf`、错误的 `iptables` 规则）都可能导致**不可逆**的服务中断或数据丢失，且难以快速回滚。
        *   **解决方案**：从工程层面隔离不同的阶段，问题分析阶段严格不允许执行高危操作，问题修复阶段要让用户严格确认，同时生成的修复要有相应的回滚机制。
        *   <TODO>
    *   **数据噪声与分析难点 (Data Noise)**
        *   **问题描述**：海量日志（GB/TB 级）难以人工处理，且通用模型直接分析大文件准确性较差（Token 限制、注意力分散）。
        *   **解决方案**：1. 不执行操作日志文件，通过python脚本或shell分析或标准工具； 2. 必须要要有过滤参数，优先时间段过滤，减少干扰项；3. 提供全景分析：类似给日志建立索引，全局都有哪些报错信息，类型都是哪些；让模型决策哪些是可能的原因。
        *   <TODO>
    *   **结果确定性 (Determinism)**
        *   **问题描述**：运维场景追求唯一且确定的“真值”（如服务状态必然是 Up 或 Down），严禁模棱两可。这与创意生成类任务（结果开放、多解）形成鲜明对比，任何模糊结论都可能导致自动化流程的误判，因此对准确性的要求极高。
        *   **解决方案**：没有很好的解决办法，依赖模型和skills的能力，一些提高skills的方式：1. 至少包含3~5个典型的案例；2. 优先解决报错问题，模型在根据报错信息推断方面效果较好，但对模凌两可的问题效果仍然较差，性能优化问题特别是没有报错信息的场景准确性需要详细打磨。
        *   <TODO>

## 5. 基于 Witty 框架的 Skills 全生命周期
> **本章目标**：基于 Witty 框架，解析 Skills 的全生命周期管理（生成、执行、优化、链路跟踪）。

*   **5.1 基于案例的 Skills 生成 (Case-based Skills Generation)**
    *   **核心工具**：主要使用 [witty-skill-insight](https://atomgit.com/openeuler/witty-skill-insight)
    *   **流程**：利用 Witty 平台的框架能力，从历史运维案例中提取特征，自动或辅助生成标准化的 Skills。
    *   <TODO>
*   **5.2 基于运维场景的运行 (Execution in Ops Scenarios)**
    *   **核心工具**：主要使用 [witty-diagnosis-agent](https://atomgit.com/openeuler/witty-diagnosis-agent)
    *   **流程**：在实际运维场景中，通过 Diagnosis Agent 加载并执行 Skills，解决具体故障或执行任务。
    *   <TODO>
*   **5.3 执行效果观测及优化分析 (Observation & Optimization)**
    *   **核心工具**：主要使用 [witty-skill-insight](https://atomgit.com/openeuler/witty-skill-insight)
    *   **流程**：对 Skills 的执行结果进行全链路跟踪与效果评估，基于数据反馈进行持续优化。
    *   <TODO>

## 6. 参考资料
*   [Agent Skills GitHub 仓库](https://github.com/agentskills/agentskills)
*   [Agent Skills 官方网站](https://agentskills.io/home)
*   [The Complete Guide to Building Skills for Claude](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
*   [Skills Best Practices](https://github.com/mgechev/skills-best-practices)
*   [Claude Skills 完全指南：让 AI 成为你的领域专家](https://zhuanlan.zhihu.com/p/1996713664087090521)
*   [Anthropic 官方 Skills 示例集](https://github.com/anthropics/skills/tree/main/skills)
*   [Skill Creator 工具 (自动生成 Skills)](https://github.com/anthropics/skills/tree/main/skills/skill-creator)
*   [Awesome Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills)
