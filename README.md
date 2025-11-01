# Claude Code 学习路径导航

![image.png](https://raw.githubusercontent.com/hacket/ObsidianOSS/master/obsidian/202510310825827.png)

中文版：

![image.png](https://raw.githubusercontent.com/hacket/ObsidianOSS/master/obsidian/202510310825475.png)

## 🎯 学习路径概述

本知识库按照 Claude Code 官方学习路径组织,从基础到高级,循序渐进地帮助你掌握 Claude Code 的各项功能。

## 📚 六大学习级别

### 🟨 [Level 1: Core CLI - 核心 CLI](Level-1-Core-CLI/README.md)

掌握 Claude Code 的核心命令行界面功能。

**核心笔记:**
- [安装与配置](Level-1-Core-CLI/01-Claude%20Code安装与配置.md)
- [基本命令](Level-1-Core-CLI/01-Claude%20Code%20基本用法.md)
- [常用命令参考](Level-1-Core-CLI/02-Cladue%20Code常用命令参考.md)
- [工作流实践](Level-1-Core-CLI/01-Claude%20Code工作流实践.md)

---

### 🟧 [Level 2: Configuration & Customization - 配置与自定义](Level-2-Configuration/README.md)

深入配置 Claude Code,打造个性化开发环境。

**核心笔记:**
- [CLAUDE.md 使用指南](Level-2-Configuration/01-CLAUDE.md使用指南.md)
- [使用其他大模型](Level-2-Configuration/01-Claude%20Code使用其他大模型.md)

---

### 🟥 [Level 3: Extension Systems - 扩展系统](Level-3-Extension-Systems/README.md)

掌握 Claude Code 的高级扩展能力。

**核心笔记:**
- [Sub Agents 使用指南](Level-3-Extension-Systems/01-Sub-Agents使用指南.md)
- [MCP 基础与使用](Level-3-Extension-Systems/01-MCP基础与使用.md)
- [Hooks 完全指南](Level-3-Extension-Systems/01-Hooks完全指南.md)
- [Claude Agent Skill](Level-3-Extension-Systems/Claude%20Agent%20Skill.md)
- [Claude Code Skills总结](Level-3-Extension-Systems/Claude%20Code%20Skills总结.md)
- [进阶用法](Level-3-Extension-Systems/Cladue%20Code进阶用法.md)

---

### 🟪 [Level 4: Programmatic Usage - 编程式使用](Level-4-Programmatic/README.md)

学习以编程方式集成和自动化 Claude Code。

**学习模块:**
- 4.1 Headless Mode - 无头模式
- 4.2 Python SDK
- 4.3 TypeScript SDK
- 4.4 GitHub Actions

---

### 🟩 [Level 5: Enterprise Deployment - 企业部署](Level-5-Enterprise/README.md)

企业级部署、安全和监控。

**学习模块:**
- 5.1 IAM - 身份访问管理
- 5.2 Cloud Providers - 云服务商
- 5.3 Network Config - 网络配置
- 5.4 Security - 安全配置
- 5.5 Monitoring - 监控分析

---

### 🟦 [Level 6: Best Practices - 最佳实践](Level-6-Best-Practices/README.md)

最佳实践、开源项目汇总和个人使用经验等扩展材料。

**主要资源:**
- [官方最佳实践](Level-6-Best-Practices/01-Claude%20Code%20官方最佳实践.md)
- [个人使用总结](Level-6-Best-Practices/04-Claude%20Code个人使用总结.md)
- [使用技巧](Level-6-Best-Practices/03-Claude%20Code%20使用技巧.md)
- [开源项目汇总](Level-6-Best-Practices/01-Claude%20Code%20开源项目汇总.md)
- [SuperClaude V3](Level-6-Best-Practices/02-SuperClaude-V3.md)
- [SuperClaude V4](Level-6-Best-Practices/03-SuperClaude-V4.md)

---

## 🗺️ 学习建议

### 初学者路径

1. 从 **Level 1** 开始,完成安装和基本使用
2. 学习 **Level 2** 的配置,打造舒适的开发环境
3. 阅读 **Level 6** 中的使用技巧

### 进阶开发者路径

1. 重点学习 **Level 3** 的扩展系统
2. 掌握 MCP 集成和 Hooks 系统
3. 参考 **Level 6** 中的开源项目

### 企业用户路径

1. 快速浏览 Level 1-3
2. 深入学习 **Level 4** 的编程式集成
3. 重点关注 **Level 5** 的企业部署和安全

---

## 📝 学习进度跟踪

你可以在每个级别的 README 文件中找到学习检查点,帮助你确认是否已掌握该级别的核心知识。

---

## 🔗 相关资源

- 📚 [Claude Code 官方文档](https://docs.anthropic.com/zh-CN/docs/claude-code/overview)
- 🐙 [Claude Code GitHub](https://github.com/anthropics/claude-code)
- 🚀 [Claude Skills 官方仓库](https://github.com/anthropics/skills)
- 🎓 [Anthropic Courses](https://anthropic.skilljar.com/)

---

## 🛠️ 项目配置

本项目已配置 [`CLAUDE.md`](CLAUDE.md) 文件，为 Claude Code 提供项目上下文和开发指导。

### 开发规范
- 使用中文内容，专业术语保留英文
- 文档采用 Obsidian 格式，支持 wikilink 内部链接
- 每个级别文件夹建议不超过 8 个文件
- 遵循渐进式学习路径设计

### 🚀 内置 Skills

项目还包含了丰富的 Claude Code Skills，扩展了 Claude 的专业能力。这些技能来自 [Claude 官方 Skills 仓库](https://github.com/anthropics/skills)：

#### 🎨 创意与设计
- **algorithmic-art** - 算法艺术生成，使用 p5.js 创建参数化艺术作品
- **artifacts-builder** - 复杂 HTML artifacts 构建工具，支持 React + Tailwind CSS
- **canvas-design** - 专业视觉设计工具，支持 PNG/PDF 输出
- **brand-guidelines** - Anthropic 官方品牌风格应用
- **theme-factory** - 主题样式工具包

#### 📄 文档处理
- **docx** - Word 文档创建、编辑和分析，支持修订跟踪
- **pdf** / **pdf-processing-pro** - PDF 处理工具，支持表单、OCR、批量操作
- **xlsx** - Excel 电子表格处理
- **pptx** - PowerPoint 演示文稿处理

#### 🛠️ 开发工具
- **skill-creator** - 新技能开发指南
- **mcp-builder** - MCP 服务器构建工具
- **webapp-testing** - 本地 Web 应用测试工具包
- **template-skill** - 技能模板

#### 🔍 信息验证工具
- **source-tracing** - 信息溯源技能，从高熵的二手内容中回溯到低熵、权威、原始的信息源

#### 📊 企业工具
- **internal-comms** - 内部沟通文档模板
- **slack-gif-creator** - Slack 动画 GIF 创建工具
- **video-downloader** - 视频下载工具

使用方法：输入 `/skill` 命令或直接调用相关技能名称即可使用。

### 📖 Skills 使用文档

详细的 Skill 使用指南请查看：

- 🔍 [**Source Tracing - 信息溯源技能**](skills/docs/source-tracing.md) ⭐ **重点推荐**
  > 追溯信息源，一层一层往下挖，去读、去看最原始的材料，就已经领先了至少 99% 的人。

---

## 📮 反馈与贡献

如果你在学习过程中发现问题或有改进建议,欢迎提出!

---

**祝你学习愉快! 🚀**

- [x.com/dani\_avila7/status/1983331447571124382?t=Bk-zOjvpdo7sP5hHeblW-A&s=09](https://x.com/dani_avila7/status/1983331447571124382?t=Bk-zOjvpdo7sP5hHeblW-A&s=09)
