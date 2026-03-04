---
name: pptx
description: "任何时候只要涉及 .pptx 文件（无论是作为输入、输出还是两者兼有），请使用此技能。这包括：创建幻灯片、演示文稿或 pitch decks；读取、解析或从任何 .pptx 文件中提取文本（即使提取的内容将用于其他地方，如电子邮件或摘要）；编辑、修改或更新现有的演示文稿；合并或拆分幻灯片文件；使用模板、布局、演讲者备注或评论。只要用户提到“deck”、“slides”、“presentation”或引用 .pptx 文件名，无论他们计划随后如何处理内容，都应触发此技能。如果需要打开、创建或接触 .pptx 文件，请使用此技能。"
license: Proprietary. LICENSE.txt has complete terms
---

# PPTX Skill (PPTX 技能)

## 快速参考 (Quick Reference)

| 任务 | 指南 |
|------|-------|
| 读取/分析内容 | `python -m markitdown presentation.pptx` |
| 编辑或基于模板创建 | 阅读 [editing.md](editing.md) |
| 从头创建 | 阅读 [pptxgenjs.md](pptxgenjs.md) |

---

## 读取内容 (Reading Content)

```bash
# 文本提取
python -m markitdown presentation.pptx

# 视觉概览
python scripts/thumbnail.py presentation.pptx

# 原始 XML
python scripts/office/unpack.py presentation.pptx unpacked/
```

---

## 编辑工作流 (Editing Workflow)

**阅读 [editing.md](editing.md) 获取完整详情。**

1. 使用 `thumbnail.py` 分析模板
2. 解包 (Unpack) → 操作幻灯片 → 编辑内容 → 清理 (Clean) → 打包 (Pack)

---

## 从头创建 (Creating from Scratch)

**阅读 [pptxgenjs.md](pptxgenjs.md) 获取完整详情。**

当没有模板或参考演示文稿可用时使用。

---

## 设计理念 (Design Ideas)

**不要创建无聊的幻灯片。** 白色背景上的普通项目符号不会打动任何人。请考虑以下列表中的想法用于每一张幻灯片。

### 开始之前 (Before Starting)

- **挑选一个大胆、基于内容的配色方案**：配色方案应该感觉像是专门为这个主题设计的。如果将你的颜色换到一个完全不同的演示文稿中仍然“行得通”，说明你的选择不够具体。
- **主次分明 (Dominance over equality)**：一种颜色应占主导地位（60-70% 的视觉比重），配以 1-2 种辅助色调和一种鲜明的强调色。永远不要让所有颜色比重相等。
- **深浅对比 (Dark/light contrast)**：标题页 + 结束页使用深色背景，内容页使用浅色背景（“三明治”结构）。或者全程使用深色以获得高级感。
- **坚持一个视觉母题 (Commit to a visual motif)**：挑选一个独特的元素并重复使用——圆形图片框、彩色圆圈中的图标、粗线条的单边框。在每一张幻灯片中都沿用它。

### 配色方案 (Color Palettes)

选择与你的主题相匹配的颜色——不要默认使用通用的蓝色。使用这些配色方案作为灵感：

| 主题 | 主色 (Primary) | 辅色 (Secondary) | 强调色 (Accent) |
|-------|---------|-----------|--------|
| **午夜行政 (Midnight Executive)** | `1E2761` (海军蓝) | `CADCFC` (冰蓝) | `FFFFFF` (白) |
| **森林与苔藓 (Forest & Moss)** | `2C5F2D` (森林绿) | `97BC62` (苔藓绿) | `F5F5F5` (奶油色) |
| **珊瑚活力 (Coral Energy)** | `F96167` (珊瑚红) | `F9E795` (金黄) | `2F3C7E` (海军蓝) |
| **暖陶土 (Warm Terracotta)** | `B85042` (陶土红) | `E7E8D1` (沙色) | `A7BEAE` (鼠尾草绿) |
| **海洋渐变 (Ocean Gradient)** | `065A82` (深蓝) | `1C7293` (蓝绿) | `21295C` (午夜蓝) |
| **极简炭灰 (Charcoal Minimal)** | `36454F` (炭灰) | `F2F2F2` (米白) | `212121` (黑) |
| **信任青色 (Teal Trust)** | `028090` (青色) | `00A896` (海泡绿) | `02C39A` (薄荷绿) |
| **浆果奶油 (Berry & Cream)** | `6D2E46` (浆果红) | `A26769` (干枯玫瑰) | `ECE2D0` (奶油色) |
| **宁静鼠尾草 (Sage Calm)** | `84B59F` (鼠尾草绿) | `69A297` (桉树绿) | `50808E` (板岩蓝) |
| **大胆樱桃 (Cherry Bold)** | `990011` (樱桃红) | `FCF6F5` (米白) | `2F3C7E` (海军蓝) |

### 每一张幻灯片 (For Each Slide)

**每张幻灯片都需要一个视觉元素**——图片、图表、图标或形状。纯文本的幻灯片容易被遗忘。

**布局选项：**
- 双栏（左侧文本，右侧插图）
- 图标 + 文本行（彩色圆圈中的图标，粗体标题，下方描述）
- 2x2 或 2x3 网格（一侧图片，另一侧内容块网格）
- 半版图片（占据整个左侧或右侧）配内容覆盖

**数据显示：**
- 大型数据标注（60-72pt 的大数字，下方配小标签）
- 对比栏（之前/之后，优点/缺点，并排选项）
- 时间线或流程图（编号步骤，箭头）

**视觉润色：**
- 章节标题旁的小彩色圆圈图标
- 关键统计数据或标语使用斜体强调文本

### 排版 (Typography)

**选择有趣的字体搭配**——不要默认使用 Arial。挑选一个有个性的标题字体，并搭配一个干净的正文字体。

| 标题字体 | 正文字体 |
|-------------|-----------|
| Georgia | Calibri |
| Arial Black | Arial |
| Calibri | Calibri Light |
| Cambria | Calibri |
| Trebuchet MS | Calibri |
| Impact | Arial |
| Palatino | Garamond |
| Consolas | Calibri |

| 元素 | 大小 |
|---------|------|
| 幻灯片标题 | 36-44pt 粗体 |
| 章节标题 | 20-24pt 粗体 |
| 正文文本 | 14-16pt |
| 说明文字 | 10-12pt 柔和 |

### 间距 (Spacing)

- 最小 0.5 英寸边距
- 内容块之间 0.3-0.5 英寸
- 留出呼吸空间——不要填满每一寸

### 避免（常见错误） (Avoid - Common Mistakes)

- **不要重复相同的布局**——在幻灯片之间变换列、卡片和标注
- **不要居中正文文本**——左对齐段落和列表；仅居中标题
- **不要吝啬大小对比**——标题需要 36pt+ 才能从 14-16pt 的正文中脱颖而出
- **不要默认使用蓝色**——选择反映特定主题的颜色
- **不要随机混合间距**——选择 0.3" 或 0.5" 间隙并一致使用
- **不要只设计一张幻灯片而让其余的平淡无奇**——全力以赴或保持全程简单
- **不要创建纯文本幻灯片**——添加图片、图标、图表或视觉元素；避免纯标题 + 项目符号
- **不要忘记文本框填充**——当将线条或形状与文本边缘对齐时，在文本框上设置 `margin: 0` 或偏移形状以考虑填充
- **不要使用低对比度元素**——图标和文本需要与背景形成强烈对比；避免浅色背景上的浅色文本或深色背景上的深色文本
- **永远不要在标题下使用强调线**——这是 AI 生成幻灯片的标志；使用留白或背景颜色代替

---

## 质量保证 (QA - Required)

**假设存在问题。你的工作是找到它们。**

你的第一次渲染几乎从未正确过。将 QA 视为寻找 Bug，而不是确认步骤。如果你在第一次检查中发现零问题，那你找得不够仔细。

### 内容 QA (Content QA)

```bash
python -m markitdown output.pptx
```

检查缺失的内容、拼写错误、错误的顺序。

**使用模板时，检查残留的占位符文本：**

```bash
python -m markitdown output.pptx | grep -iE "xxxx|lorem|ipsum|this.*(page|slide).*layout"
```

如果 grep 返回结果，在宣布成功之前修复它们。

### 视觉 QA (Visual QA)

**⚠️ 使用子智能体 (SUBAGENTS)**——即使只有 2-3 张幻灯片。你一直盯着代码看，只会看到你期望看到的，而不是实际存在的。子智能体有全新的视角。

将幻灯片转换为图片（见 [转换为图片](#converting-to-images)），然后使用此提示词：

```
视觉检查这些幻灯片。假设存在问题——找到它们。

寻找：
- 重叠元素（文本穿过形状，线条穿过文字，堆叠元素）
- 文本溢出或在边缘/框边界处被切断
- 装饰线定位为单行文本，但标题换行成了两行
- 来源引用或页脚与上方内容冲突
- 元素太近（< 0.3" 间隙）或卡片/部分几乎接触
- 不均匀的间隙（一处有大片空白，另一处拥挤）
- 幻灯片边缘边距不足（< 0.5"）
- 列或类似元素未一致对齐
- 低对比度文本（例如，奶油色背景上的浅灰色文本）
- 低对比度图标（例如，深色背景上的深色图标，没有对比鲜明的圆圈）
- 文本框太窄导致过度换行
- 残留的占位符内容

对于每张幻灯片，列出问题或关注区域，即使是轻微的。

阅读并分析这些图片：
1. /path/to/slide-01.jpg (预期: [简要描述])
2. /path/to/slide-02.jpg (预期: [简要描述])

报告发现的所有问题，包括轻微问题。
```

### 验证循环 (Verification Loop)

1. 生成幻灯片 → 转换为图片 → 检查
2. **列出发现的问题**（如果未发现，更批判性地再看一遍）
3. 修复问题
4. **重新验证受影响的幻灯片**——一个修复通常会产生另一个问题
5. 重复直到完整通过未发现新问题

**在完成至少一个修复和验证周期之前，不要宣布成功。**

---

## 转换为图片 (Converting to Images)

将演示文稿转换为单独的幻灯片图片以进行视觉检查：

```bash
python scripts/office/soffice.py --headless --convert-to pdf output.pptx
pdftoppm -jpeg -r 150 output.pdf slide
```

这将创建 `slide-01.jpg`, `slide-02.jpg` 等。

要在修复后重新渲染特定幻灯片：

```bash
pdftoppm -jpeg -r 150 -f N -l N output.pdf slide-fixed
```

---

## 依赖项 (Dependencies)

- `pip install "markitdown[pptx]"` - 文本提取
- `pip install Pillow` - 缩略图网格
- `npm install -g pptxgenjs` - 从头创建
- LibreOffice (`soffice`) - PDF 转换（通过 `scripts/office/soffice.py` 为沙箱环境自动配置）
- Poppler (`pdftoppm`) - PDF 转图片
