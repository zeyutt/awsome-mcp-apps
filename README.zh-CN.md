# Awesome MCP Apps [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | 简体中文

> MCP Apps（模型上下文协议应用）精选列表 - AI 助手的交互式用户界面

MCP Apps 扩展了模型上下文协议，为 AI 对话带来丰富的交互式 UI 功能。工具不再局限于返回文本，而是可以提供交互式仪表盘、表单、可视化等内容 - 全部安全地在聊天界面中渲染。

## 目录

- [什么是 MCP Apps？](#什么是-mcp-apps)
- [官方资源](#官方资源)
- [官方示例](#官方示例)
- [社区应用](#社区应用)
- [开发工具](#开发工具)
- [教程与指南](#教程与指南)
- [使用场景](#使用场景)
- [文档](#文档)
- [贡献](#贡献)

## 什么是 MCP Apps？

MCP Apps 是模型上下文协议的标准化扩展，允许工具提供交互式用户界面而不仅仅是文本响应。主要特性：

- **丰富的 UI**：在对话中直接嵌入交互式仪表盘、图表、表单等
- **双向通信**：UI 可以通过 JSON-RPC 与 AI 宿主端发送和接收数据
- **安全性**：所有代码在沙箱化的 iframe 中运行以实现隔离
- **框架无关**：支持 React、Vue、Svelte、原生 JS 等

### 架构

MCP Apps 基于两个核心原语构建：

1. **带有 UI 元数据的工具**：工具声明一个 `_meta.ui.resourceUri` 字段指向 UI 资源
2. **UI 资源**：通过 `ui://` 协议提供，包含 HTML/JavaScript 包

```json
{
  "name": "visualize_data",
  "description": "交互式数据可视化",
  "_meta": {
    "ui": {
      "resourceUri": "ui://charts/interactive"
    }
  }
}
```

### 工作原理

1. 工具使用 `ui://` URI 声明 UI 资源
2. 宿主端获取并在沙箱化的 iframe 中渲染 UI
3. UI 通过 `postMessage` (JSON-RPC) 与宿主端通信
4. 用户交互实时更新对话上下文

## 官方资源

### 文档与规范

- [官方 MCP Apps 文档](https://modelcontextprotocol.io/docs/extensions/apps) - 完整技术文档
- [MCP Apps 规范](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/draft/apps.mdx) - SEP-1865 规范
- [MCP Apps SDK (@modelcontextprotocol/ext-apps)](https://www.npmjs.com/package/@modelcontextprotocol/ext-apps) - 官方 npm 包
- [API 文档](https://modelcontextprotocol.github.io/ext-apps/api/) - 完整 API 参考

### 官方公告与博客文章

- [MCP Apps - 为 MCP 客户端带来 UI 功能](http://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps/) - 官方发布公告
- [MCP Apps：通过交互式用户界面扩展服务器](http://blog.modelcontextprotocol.io/posts/2025-11-21-mcp-apps/) - 初始介绍
- [MCP Apps 现已正式发布](https://modelcontextprotocol.info/blog/mcp-apps-ui-capabilities/) - 社区报道

### GitHub 仓库

- [modelcontextprotocol/ext-apps](https://github.com/modelcontextprotocol/ext-apps) - 官方规范和 SDK 仓库

## 官方示例

来自官方 `@modelcontextprotocol/ext-apps` 仓库的示例：

### 框架启动模板

- **basic-server-vanillajs** - 原生 JavaScript 启动模板
- **basic-server-react** - React 启动模板
- **basic-server-vue** - Vue.js 启动模板
- **basic-server-svelte** - Svelte 启动模板
- **basic-server-preact** - Preact 启动模板
- **basic-server-solid** - Solid.js 启动模板
- **basic-host** - 用于测试 MCP Apps 的基础宿主端实现

### 完整功能示例

- **threejs-server** - 使用 Three.js 的 3D 可视化
- **map-server** - 交互式地理地图
- **pdf-server** - 带注释的内联 PDF 查看器
- **system-monitor-server** - 实时系统健康和指标仪表盘
- **sheet-music-server** - 交互式乐谱和播放
- **say-server** - 带卡拉 OK 高亮的流式文本转语音

## 社区应用

### 数据可视化

- [mcp-app-yfinance-example](https://github.com/trsdn/mcp-app-yfinance-example) - 使用 yfinance 数据的交互式股票图表 UI，Python 后端。聊天中的实时金融数据可视化。
- [mcp-chart-app-demo](https://github.com/samueltauil/mcp-chart-app-demo) - 使用 React 和 Chart.js 的交互式图表。展示 MCP Apps SDK 的丰富 UI 体验。

### 生产力工具

- [ideate-chatgpt-app](https://github.com/umairkhancis/ideate-chatgpt-app) - ChatGPT 中的"第二大脑"想法管理系统。通过交互式 UI 小部件捕获、组织和管理想法。
- [embroker-mcp-app](https://github.com/evansluccas/embroker-mcp-app) - 使用 React 构建的交互式保险覆盖选择 UI。通过引导工作流简化产品选择。

### 开发工具

- [sprite-mcp-server](https://github.com/Anansitrading/sprite-mcp-server) - 带交互式 UI 控制和监控的 Sprite VM 管理 MCP 服务器。

### 旅游与酒店

- [hotelplanner-chatgpt-app](https://github.com/nvkaragiannis/hotelplanner-chatgpt-app) - 具有智能搜索和交互式 UI 的团体旅行预订。OpenAI Apps SDK 实现。

### 设计工具

- [mcpapp-colorpicker](https://github.com/elbruno/mcpapp-colorpicker) - 使用 .NET 构建的带丰富 UI 的交互式颜色选择器。在 AI 对话中进行视觉颜色选择。

### 其他

<!-- 在此添加其他社区 MCP Apps -->

## 开发工具

### 框架与库

- [@modelcontextprotocol/ext-apps](https://www.npmjs.com/package/@modelcontextprotocol/ext-apps) - 用于构建 MCP Apps 的官方 SDK
- [MCP-UI Playground](https://mcpui.dev/) - 用于测试 MCP Apps 的交互式游乐场

### 模板与样板

- [官方仓库](https://github.com/modelcontextprotocol/ext-apps/tree/main/examples)中的 React、Vue、Svelte 和其他框架模板

## 教程与指南

### 入门

- [官方 MCP Apps 指南](https://modelcontextprotocol.io/docs/extensions/apps) - 完整的设置和开发指南
- [MCP Apps 交互式 UI 指南](https://deepwiki.com/FlorianBruniaux/claude-code-ultimate-guide/6.6-mcp-apps-interactive-ui) - 综合教程

### 架构与最佳实践

- [MCP Apps：交互式 AI 界面的新标准](https://www.unite.ai/what-are-mcp-apps-the-new-standard-turning-ai-responses-into-interactive-interfaces/) - 概述文章
- [MCP Apps 来了：在 AI 客户端中渲染交互式 UI](https://workos.com/blog/2026-01-27-mcp-apps) - 技术深入探讨

## 使用场景

### 数据探索
交互式仪表盘，用户可以筛选、排序和深入数据，无需重复的文本命令。

**示例**：销售分析工具，带有显示按地区收入的交互式图表，可深入查看账户详细信息。

### 配置向导
多步骤表单，根据用户选择自适应条件字段。

**示例**：部署配置工具，为生产环境显示不同的安全选项，为预发布环境显示不同选项。

### 文档审核
带注释和审批功能的内联文档查看器。

**示例**：合同分析工具显示 PDF，高亮关键条款，允许直接批准或标记。

### 实时监控
自动更新的实时仪表盘，无需新的提示。

**示例**：服务器健康监控显示实时指标、日志和警报。

### 交互式表单
具有验证、自动完成和动态字段更新的复杂数据输入。

**示例**：具有条件逻辑的多步骤调查或申请表。

### 教育工具
交互式教程、测验和学习体验。

**示例**：用于学习编程的代码游乐场，具有实时执行功能。

## 文档

- [入门指南](docs/getting-started.md) - 构建第一个 MCP App 的完整教程
- [常见问题](docs/FAQ.md) - 关于 MCP Apps 的常见问题

## 贡献

欢迎贡献！请先阅读[贡献指南](CONTRIBUTING.md)。

### 如何贡献

1. **添加新的 MCP App**：在适当的类别中提交包含应用详细信息的 PR
2. **改进文档**：帮助使指南更清晰、更全面
3. **分享使用案例**：描述使用 MCP Apps 的有趣方式
4. **报告问题**：让我们知道损坏的链接或过时的信息

### 提交指南

添加新的 MCP App 时，请包括：
- 带有仓库或网站链接的应用名称
- 简要描述（1-2 句话）
- 关键功能或使用案例
- 屏幕截图或演示（如果有）
- 许可证信息

## 许可证

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0)

在法律允许的范围内，贡献者已放弃对本作品的所有版权和相关权利。
