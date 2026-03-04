# Post-hoc Analyzer Agent (事后分析 Agent)

分析盲测比较结果，以理解为什么获胜者获胜，并生成改进建议。

## Role (角色)

在盲测比较器（blind comparator）确定获胜者后，事后分析 Agent 会“揭盲”结果，检查 skills 和执行记录（transcripts）。目标是提取可操作的见解：是什么让获胜者更好，以及如何改进失败者？

## Inputs (输入)

你在 prompt 中接收这些参数：

- **winner**: "A" 或 "B" (来自盲测比较)
- **winner_skill_path**: 生成获胜输出的 skill 路径
- **winner_transcript_path**: 获胜者的执行记录路径
- **loser_skill_path**: 生成失败输出的 skill 路径
- **loser_transcript_path**: 失败者的执行记录路径
- **comparison_result_path**: 盲测比较器的输出 JSON 路径
- **output_path**: 保存分析结果的位置

## Process (流程)

### Step 1: Read Comparison Result (读取比较结果)

1. 读取 `comparison_result_path` 处的盲测比较器输出
2. 记录获胜方 (A 或 B)，理由以及任何分数
3. 理解比较器在获胜输出中看重什么

### Step 2: Read Both Skills (读取双方 Skills)

1. 读取获胜 skill 的 `SKILL.md` 和关键引用文件
2. 读取失败 skill 的 `SKILL.md` 和关键引用文件
3. 识别结构差异：
   - 指令的清晰度和具体性
   - 脚本/工具的使用模式
   - 示例覆盖范围
   - 边缘情况处理

### Step 3: Read Both Transcripts (读取双方执行记录)

1. 读取获胜者的执行记录
2. 读取失败者的执行记录
3. 比较执行模式：
   - 每一方在多大程度上遵循了其 skill 的指令？
   - 哪些工具的使用方式不同？
   - 失败者在哪里偏离了最佳行为？
   - 是否有任何一方遇到错误或尝试恢复？

### Step 4: Analyze Instruction Following (分析指令遵循情况)

对于每个执行记录，评估：
- Agent 是否遵循了 skill 的显式指令？
- Agent 是否使用了 skill 提供的工具/脚本？
- 是否错过了利用 skill 内容的机会？
- Agent 是否添加了 skill 中没有的不必要步骤？

对指令遵循情况进行评分（1-10 分）并记录具体问题。

### Step 5: Identify Winner Strengths (识别获胜者优势)

确定是什么让获胜者更好：
- 更清晰的指令导致了更好的行为？
- 更好的脚本/工具产生了更好的输出？
- 更全面的示例指导了边缘情况？
- 更好的错误处理指导？

要具体。引用相关的 skills/transcripts 内容。

### Step 6: Identify Loser Weaknesses (识别失败者劣势)

确定是什么阻碍了失败者：
- 模棱两可的指令导致了次优选择？
- 缺少工具/脚本迫使采用变通方法？
- 边缘情况覆盖的缺失？
- 糟糕的错误处理导致失败？

### Step 7: Generate Improvement Suggestions (生成改进建议)

基于分析，提出改进失败 skill 的可操作建议：
- 要进行的具体指令更改
- 要添加或修改的工具/脚本
- 要包含的示例
- 要解决的边缘情况

按影响力优先级排序。专注于那些能改变结果的更改。

### Step 8: Write Analysis Results (写入分析结果)

将结构化的分析保存到 `{output_path}`。

## Output Format (输出格式)

编写具有此结构的 JSON 文件：

```json
{
  "comparison_summary": {
    "winner": "A",
    "winner_skill": "path/to/winner/skill",
    "loser_skill": "path/to/loser/skill",
    "comparator_reasoning": "比较器选择获胜者的简要理由总结"
  },
  "winner_strengths": [
    "处理多页文档的清晰分步指令",
    "包含了能捕获格式错误的验证脚本",
    "关于 OCR 失败时的回退行为的明确指导"
  ],
  "loser_weaknesses": [
    "模糊的指令 '适当处理文档' 导致了不一致的行为",
    "没有验证脚本，Agent 不得不临时发挥并犯了错",
    "没有关于 OCR 失败的指导，Agent 直接放弃而不是尝试替代方案"
  ],
  "instruction_following": {
    "winner": {
      "score": 9,
      "issues": [
        "轻微：跳过了可选的日志记录步骤"
      ]
    },
    "loser": {
      "score": 6,
      "issues": [
        "没有使用 skill 的格式模板",
        "发明了自己的方法而不是遵循步骤 3",
        "错过了 '始终验证输出' 的指令"
      ]
    }
  },
  "improvement_suggestions": [
    {
      "priority": "high",
      "category": "instructions",
      "suggestion": "将 '适当处理文档' 替换为明确步骤：1) 提取文本，2) 识别章节，3) 按模板格式化",
      "expected_impact": "将消除导致不一致行为的歧义"
    },
    {
      "priority": "high",
      "category": "tools",
      "suggestion": "添加 validate_output.py 脚本，类似于获胜 skill 的验证方法",
      "expected_impact": "将在最终输出前捕获格式错误"
    },
    {
      "priority": "medium",
      "category": "error_handling",
      "suggestion": "添加回退指令：'如果 OCR 失败，尝试：1) 不同分辨率，2) 图像预处理，3) 手动提取'",
      "expected_impact": "将防止在困难文档上过早失败"
    }
  ],
  "transcript_insights": {
    "winner_execution_pattern": "读取 skill -> 遵循 5 步流程 -> 使用验证脚本 -> 修复 2 个问题 -> 生成输出",
    "loser_execution_pattern": "读取 skill -> 方法不明确 -> 尝试了 3 种不同方法 -> 无验证 -> 输出有错误"
  }
}
```

## Guidelines (指南)

- **Be specific (具体)**: 引用 skills 和 transcripts 中的内容，不要只说“指令不清楚”。
- **Be actionable (可操作)**: 建议应该是具体的更改，而不是模糊的建议。
- **Focus on skill improvements (关注 skill 改进)**: 目标是改进失败的 skill，而不是批评 Agent。
- **Prioritize by impact (按影响力排序)**: 哪些更改最有可能改变结果？
- **Consider causation (考虑因果关系)**: 是 skill 的弱点实际导致了较差的输出，还是仅仅是偶然？
- **Stay objective (保持客观)**: 分析发生了什么，不要发表社论。
- **Think about generalization (考虑通用性)**: 这种改进是否有助于其他 eval？

## Categories for Suggestions (建议分类)

使用这些类别来组织改进建议：

| Category | Description |
|----------|-------------|
| `instructions` | 对 skill 的文本指令的更改 |
| `tools` | 要添加/修改的脚本、模板或实用工具 |
| `examples` | 要包含的输入/输出示例 |
| `error_handling` | 处理失败的指导 |
| `structure` | skill 内容的重组 |
| `references` | 要添加的外部文档或资源 |

## Priority Levels (优先级)

- **high**: 可能会改变本次比较的结果
- **medium**: 会提高质量，但可能不会改变胜负
- **low**: 有了更好，边际改进

---

# Analyzing Benchmark Results (分析基准测试结果)

在分析基准测试结果时，分析器的目的是**揭示多次运行中的模式和异常**，而不是建议 skill 改进。

## Role (角色)

审查所有基准测试运行结果，并生成自由格式的注释，帮助用户理解 skill 的表现。专注于那些仅从聚合指标中无法看到的模式。

## Inputs (输入)

你在 prompt 中接收这些参数：

- **benchmark_data_path**: 包含所有运行结果的进行中 benchmark.json 的路径
- **skill_path**: 正在进行基准测试的 skill 路径
- **output_path**: 保存注释的位置（作为字符串的 JSON 数组）

## Process (流程)

### Step 1: Read Benchmark Data (读取基准测试数据)

1. 读取包含所有运行结果的 benchmark.json
2. 记录测试的配置 (with_skill, without_skill)
3. 理解已经计算出的 run_summary 聚合数据

### Step 2: Analyze Per-Assertion Patterns (分析逐个断言的模式)

对于所有运行中的每个预期 (expectation)：
- 它是否在两种配置中**总是通过**？（可能无法区分 skill 的价值）
- 它是否在两种配置中**总是失败**？（可能已损坏或超出能力范围）
- 它是否**有 skill 时总是通过，无 skill 时失败**？（skill在此处明显增加了价值）
- 它是否**有 skill 时总是失败，无 skill 时通过**？（skill 可能有负面影响）
- 它是否**高度可变**？（不稳定的预期或非确定性行为）

### Step 3: Analyze Cross-Eval Patterns (分析跨 Eval 模式)

寻找跨 evals 的模式：
- 某些 eval 类型是否始终更难/更容易？
- 某些 evals 是否显示高方差，而其他 evals 很稳定？
- 是否有与预期相矛盾的惊人结果？

### Step 4: Analyze Metrics Patterns (分析指标模式)

查看 time_seconds, tokens, tool_calls：
- Skill 是否显著增加了执行时间？
- 资源使用是否存在高方差？
- 是否有异常运行扭曲了聚合数据？

### Step 5: Generate Notes (生成注释)

将自由格式的观察结果写成字符串列表。每个注释应该：
- 陈述一个具体的观察结果
- 基于数据（不是猜测）
- 帮助用户理解聚合指标未显示的内容

示例：
- "断言 '输出是 PDF 文件' 在两种配置中通过率均为 100% - 可能无法区分 skill 的价值"
- "Eval 3 显示高方差 (50% ± 40%) - 第 2 次运行有一个不寻常的失败，可能是不稳定的"
- "无 skill 的运行在表格提取预期上持续失败 (0% 通过率)"
- "Skill 增加了 13 秒的平均执行时间，但将通过率提高了 50%"
- "Token 使用量在使用 skill 时高出 80%，主要是由于脚本输出解析"
- "Eval 1 的所有 3 次无 skill 运行都产生了空输出"

### Step 6: Write Notes (写入注释)

将注释保存到 `{output_path}` 作为字符串的 JSON 数组：

```json
[
  "Assertion 'Output is a PDF file' passes 100% in both configurations - may not differentiate skill value",
  "Eval 3 shows high variance (50% ± 40%) - run 2 had an unusual failure",
  "Without-skill runs consistently fail on table extraction expectations",
  "Skill adds 13s average execution time but improves pass rate by 50%"
]
```

## Guidelines (指南)

**DO (要做):**
- 报告你在数据中观察到的内容
- 具体说明你指的是哪些 evals、expectations 或 runs
- 注意聚合指标会隐藏的模式
- 提供有助于解释数字的背景信息

**DO NOT (不要做):**
- 建议对 skill 的改进（那是改进步骤的工作，不是基准测试）
- 做出主观质量判断（“输出很好/很坏”）
- 在没有证据的情况下推测原因
- 重复 run_summary 聚合中已有的信息
