---
banner:
date_created: Thursday, October 23rd 2025, 11:41:56 pm
date_updated: Friday, October 31st 2025, 12:10:21 am
title: Claude Code Skills总结
author: hacket
categories:
  - AI
category: ClaudeCode
tags: [AI, ClaudeCode]
toc: true
description:
dg-publish: true
dg-enable-search: true
dg-show-local-graph: true
dg-show-toc: true
dg-show-file-tree: true
image-auto-upload: true
feed: show
format: list
aliases: [Claude Code Skills]
linter-yaml-title-alias: Claude Code Skills
---

# Claude Code Skills

## Skill 应用

### 企业应用

- 品牌一致性
	- 自动套用公司的品牌颜色、字体、Logo 位置
	- 示例：「帮我做一份 Q3 报告的 PPT，用公司的品牌风格」
- 标准化文件
	- 自动生成符合公司格式的会议记录、状态报告
	- 示例：「写一份给客户的周报，用我们的标准格式」
- 工作流程自动化
	- 在项目管理工具（JIRA、Asana）中按照团队惯例创建任务
	- 执行公司特定的数据分析流程

### 创意设计

- 生成式艺术：使用 p5.js 创作艺术作品
- 设计规范：按照设计哲学创建视觉素材
- 动画制作：制作符合 Slack 尺寸限制的 GIF

### 技术开发

- 代码测试：使用 Playwright 测试网页应用
- 文件处理：创建和编辑 Excel、Word、PowerPoint、PDF
- API 整合：建立 MCP 服务器连接外部服务

### 个人应用

- 自订你的工作流程
- 让 Claude 配合你的习惯工作

### 知识管理与学习

- **深度阅读分析** ⭐新增
	- 系统化分析文章、论文和长篇内容
	- 使用批判性思维、第一性原理、系统思维等多种框架
	- 四级分析深度：Quick (15分钟) / Standard (30分钟) / Deep (60分钟) / Research (120分钟+)
	- 示例：「深度分析这篇技术博客：https://example.com/article」
	- 详见：[[Deep Reading Analyst Skill使用指南]]

## Security & Best Practices

[GitHub - travisvn/awesome-claude-skills: A curated list of awesome Claude Skills, resources, and tools for customizing Claude AI workflows — particularly Claude Code](https://github.com/travisvn/awesome-claude-skills?tab=readme-ov-file#-security--best-practices)

## Skills，MCP

### Skills 与 MCP 的对比

随着时间推移，MCP 的局限性也开始显现。其中最显著的一点是对 token 的大量消耗：众所周知，GitHub 官方 MCP 本身就消耗了巨量上下文 token，而这种消耗量越大，大模型自身就越是没有发挥实际作用的空间。

可用 CLI 命令替代 MCP 操作，Skill 也知道这点

## 创建 Skill

### 使用 skill-creator （推荐）

1. Enable the skill-creator skill in Claude
2. Ask Claude: "Use the skill-creator to help me build a skill for `[your task]`"
3. Answer the interactive questions about your workflow
4. Claude generates the complete skill structure for you

### 小结

Skills 的核心价值：**不是替代 CLAUDE.md，而是给 Claude 装上 " 工具箱 "。**

### 手动

1. 手动创建目录结构：

```shell
my-skill/
├── SKILL.md          # Main skill file with frontmatter
├── scripts/          # Optional executable scripts
│   └── helper.py
└── resources/        # Optional supporting files
    └── template.json
```

2. 创建**SKILL.md with frontmatter**:

```markdown
---
name: my-skill
description: Brief description for skill discovery (keep concise)
---

# Detailed Instructions

Claude will read these instructions when the skill is activated.

## Usage
Explain how to use this skill...

## Examples
Provide clear examples...
```

3. 添加 **executable scripts**（可选）：
	- Python, JavaScript, or other scripts Claude can execute
	- Reference them in your SKILL.md instructions
4. 本地测试
5. 分享

### Skill Best Practices

- **Keep descriptions concise** - The frontmatter `description` 用于技能发现
- **Use clear, actionable instructions** - 像人类合作者一样编写说明
- **Include examples** - 在你的 SKILL.md 中展示具体示例
- **Version your skills** - 使用 git 标签进行版本管理
- **Document dependencies** - 列出任何先决条件或所需的包
- **Test thoroughly** - 在不同场景下验证你的技能是否有效

其他：

- **脚本优先** 能用脚本处理的，别让 Claude 生成代码。
	- **确定性任务**(排序、格式转换、数据清洗)：写 Python/JS 脚本  
	- **创造性任务**(生成文案、代码架构设计)：让 Claude 发挥
- **保持专注** 一个 Skill 做一件事；别把 " 爬虫 + 清洗 + 分析 + 报表 + 邮件发送 " 塞一个 Skill，拆开，需要时 Claude 会自动组合多个 Skills。
- **多放实际例子** SKILL.md 里多放真实可运行的例子
- **附参考文档** 涉及特定库，把关键文档放进去

## 安全

### 安全 Tips

- ✅ 只用自己创建或官方的 Skills  
- ✅ 从 GitHub/插件安装前先看代码  
- ❌ 别用来路不明的 Skills  
- ❌ Skills 里别放 API 密钥

恶意 Skill 可能读取文件、执行危险命令、泄露信息。

**网上下载的 Skills，必须先检查 SKILL.md 和 scripts/所有代码。**

## 疑问

### Skill 能用 mcp? 安装和卸载 mcp?

### Skill 上下文会爆吗？

**不会。**

Skills 用渐进式加载 (Progressive Disclosure)：启动时只加载元数据 (~100 tokens)，需要时才读 SKILL.md(5k tokens 内)，脚本直接执行不占上下文。

你装 20 个 Skills，启动只占 2000 tokens。真正用到某个 Skill，才加载详细内容，用完释放。

Claude Code 的上下文窗口是 200k tokens，Skills 的渐进式加载让你可以装几十个 Skills 不用担心。

 **可以用/context 命令查看上下文使用情况，随时监控。**

## Skills 开源库

### anthropics 官方 Skills

<https://github.com/anthropics/skills>

```shell
/plugin marketplace add anthropics/skills
```

#### Document Skills

处理复杂文件格式的技能：

- **[docx](https://github.com/anthropics/skills/tree/main/document-skills/docx)**  创建、编辑和分析 Word 文档，支持修订跟踪、批注、格式保留和文本提取
- **[pdf](https://github.com/anthropics/skills/tree/main/document-skills/pdf)**  全面的 PDF 处理工具包，可用于提取文本和表格、创建新 PDF、合并/拆分文档以及处理表单
- **[pptx](https://github.com/anthropics/skills/tree/main/document-skills/pptx)**  创建、编辑和分析 PowerPoint 演示文稿，支持版式、模板、图表以及自动幻灯片生成功能
- **[xlsx](https://github.com/anthropics/skills/tree/main/document-skills/xlsx)**  创建、编辑和分析 Excel 电子表格，支持公式、格式设置、数据分析和可视化

#### Design & Creative

- **[algorithmic-art](https://github.com/anthropics/skills/tree/main/algorithmic-art)** - 使用 p5.js 结合种子随机性、流场和粒子系统创作生成艺术
- **[canvas-design](https://github.com/anthropics/skills/tree/main/canvas-design)** - 运用设计理念，以.png 和.pdf 格式设计精美的视觉艺术作品
- **[slack-gif-creator](https://github.com/anthropics/skills/tree/main/slack-gif-creator)** - 创建针对 Slack 尺寸限制优化的动画 GIF

#### Development

- **[artifacts-builder](https://github.com/anthropics/skills/tree/main/artifacts-builder)** - 使用 React、Tailwind CSS 和 shadcn/ui 组件构建复杂的 claude.ai HTML 制品
- **[mcp-builder](https://github.com/anthropics/skills/tree/main/mcp-builder)** - 创建高质量 MCP 服务器以集成外部 API 和服务的指南
- **[webapp-testing](https://github.com/anthropics/skills/tree/main/webapp-testing)** - 使用 Playwright 测试本地 Web 应用程序，以进行用户界面验证和调试

#### Communication

- **[brand-guidelines](https://github.com/anthropics/skills/tree/main/brand-guidelines)** - Apply Anthropic's official brand colors and typography to artifacts 将 Anthropic 的官方品牌颜色和字体应用于成果物
- **[internal-comms](https://github.com/anthropics/skills/tree/main/internal-comms)** - Write internal communications like status reports, newsletters, and FAQs 撰写内部沟通内容，如状态报告、新闻通讯和常见问题解答

#### Skill Creation

- **[skill-creator](https://github.com/anthropics/skills/tree/main/skill-creator)** 交互式技能创建工具，通过问答引导你构建新技能

### 三方

#### Skill_Seekers

<https://github.com/yusufkaraaslan/Skill_Seekers>

可以自动抓取文档网站、GitHub 仓库和 PDF 文件，并转换为 Claude 能直接使用的 Skill（技能包）

**主要功能：**
- 支持文档网站、GitHub 仓库、PDF 文件三种来源混合爬取；
- 自动检测文档与代码间的冲突并标注警告；
- 内置 Godot、React、Vue、Django 等 8 个常用框架预设；
- 深度代码分析，提取函数、类、方法及参数类型信息；
- 异步模式让爬取速度提升 2-3 倍；
- 本地 AI 增强功能，无需 API 费用即可生成完整指南。

**用途：** 学习新的知识，特别是小众的，新的技术

[🚀程序员福音！学习新框架从此不用看文档？Skill Seeker让Claude成为你的技术导师，CrewAI、AutoGen、LangGraph随便上，自动生成完整项目代码，告别学习曲线陡峭的噩梦！不用再啃文档了！这个工具让 Claude 秒懂任何技术框架 - AI超元域的博客](https://www.aivi.fyi/aiagents/introduce-Skill-Seeker)

#### 三方 skill 市场

- [GitHub - obra/superpowers: Claude Code superpowers: core skills library](https://github.com/obra/superpowers)
- [GitHub - travisvn/awesome-claude-skills: A curated list of awesome Claude Skills, resources, and tools for customizing Claude AI workflows — particularly Claude Code](https://github.com/travisvn/awesome-claude-skills)
