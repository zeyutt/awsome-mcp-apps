# Awesome MCP Apps [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of awesome MCP Apps (Model Context Protocol Apps) - interactive user interfaces for AI assistants

MCP Apps extend the Model Context Protocol to bring rich, interactive UI capabilities directly into AI conversations. Instead of limiting tools to text responses, MCP Apps enable interactive dashboards, forms, visualizations, and more - all rendered securely within the chat interface.

## Contents

- [What are MCP Apps?](#what-are-mcp-apps)
- [Official Resources](#official-resources)
- [Official Examples](#official-examples)
- [Community Apps](#community-apps)
- [Development Tools](#development-tools)
- [Tutorials & Guides](#tutorials--guides)
- [Use Cases](#use-cases)
- [Contributing](#contributing)

## What are MCP Apps?

MCP Apps are a standardized extension to the Model Context Protocol that allows tools to provide interactive user interfaces instead of just text responses. Key features:

- **Rich UI**: Embed interactive dashboards, charts, forms, and more directly in conversations
- **Bidirectional Communication**: UI can send and receive data from the AI host via JSON-RPC
- **Security**: All code runs in sandboxed iframes for isolation
- **Framework Agnostic**: Works with React, Vue, Svelte, vanilla JS, and more

### Architecture

MCP Apps are built on two core primitives:

1. **Tools with UI Metadata**: Tools declare a `_meta.ui.resourceUri` field pointing to a UI resource
2. **UI Resources**: Served via the `ui://` protocol, containing HTML/JavaScript bundles

```json
{
  "name": "visualize_data",
  "description": "Interactive data visualization",
  "_meta": {
    "ui": {
      "resourceUri": "ui://charts/interactive"
    }
  }
}
```

### How It Works

1. Tool declares a UI resource with `ui://` URI
2. Host fetches and renders UI in a sandboxed iframe
3. UI communicates with host via `postMessage` (JSON-RPC)
4. User interactions update conversation context in real-time

## Official Resources

### Documentation & Specifications

- [Official MCP Apps Documentation](https://modelcontextprotocol.io/docs/extensions/apps) - Complete technical documentation
- [MCP Apps Specification](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/draft/apps.mdx) - SEP-1865 specification
- [MCP Apps SDK (@modelcontextprotocol/ext-apps)](https://www.npmjs.com/package/@modelcontextprotocol/ext-apps) - Official npm package
- [API Documentation](https://modelcontextprotocol.github.io/ext-apps/api/) - Complete API reference

### Official Announcements & Blog Posts

- [MCP Apps - Bringing UI Capabilities To MCP Clients](http://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps/) - Official launch announcement
- [MCP Apps: Extending servers with interactive user interfaces](http://blog.modelcontextprotocol.io/posts/2025-11-21-mcp-apps/) - Initial introduction
- [MCP Apps Now Official](https://modelcontextprotocol.info/blog/mcp-apps-ui-capabilities/) - Community coverage

### GitHub Repository

- [modelcontextprotocol/ext-apps](https://github.com/modelcontextprotocol/ext-apps) - Official specification and SDK repository

## Official Examples

Examples from the official `@modelcontextprotocol/ext-apps` repository:

### Framework Starters

- **basic-server-vanillajs** - Vanilla JavaScript starter template
- **basic-server-react** - React starter template
- **basic-server-vue** - Vue.js starter template
- **basic-server-svelte** - Svelte starter template
- **basic-server-preact** - Preact starter template
- **basic-server-solid** - Solid.js starter template
- **basic-host** - Basic host implementation for testing MCP Apps

### Full-Featured Examples

- **threejs-server** - 3D visualization using Three.js
- **map-server** - Interactive geographic maps
- **pdf-server** - Inline PDF viewer with annotations
- **system-monitor-server** - Real-time system health and metrics dashboard
- **sheet-music-server** - Interactive music notation and playback
- **say-server** - Streaming Text-to-Speech with karaoke highlighting

## Community Apps

### Data Visualization

- [mcp-app-yfinance-example](https://github.com/trsdn/mcp-app-yfinance-example) - Interactive stock chart UI using yfinance data with Python backend. Real-time financial data visualization in chat.
- [mcp-chart-app-demo](https://github.com/samueltauil/mcp-chart-app-demo) - Interactive charts using React and Chart.js. Demonstrates rich UI experiences with the MCP Apps SDK.

### Productivity Tools

- [ideate-chatgpt-app](https://github.com/umairkhancis/ideate-chatgpt-app) - "Second brain" idea management system within ChatGPT. Capture, organize, and manage ideas with interactive UI widgets.
- [embroker-mcp-app](https://github.com/evansluccas/embroker-mcp-app) - Interactive insurance coverage selection UI built with React. Simplifies product selection with guided workflows.

### Developer Tools

- [sprite-mcp-server](https://github.com/Anansitrading/sprite-mcp-server) - MCP server for Sprite VM management with interactive UI controls and monitoring.

### Travel & Hospitality

- [hotelplanner-chatgpt-app](https://github.com/nvkaragiannis/hotelplanner-chatgpt-app) - Group travel booking with smart search and interactive UI. OpenAI Apps SDK implementation.

### Design Tools

- [mcpapp-colorpicker](https://github.com/elbruno/mcpapp-colorpicker) - Interactive color picker with rich UI built in .NET. Visual color selection within AI conversations.

### Other

<!-- Add other community MCP Apps here -->

## Development Tools

### Frameworks & Libraries

- [@modelcontextprotocol/ext-apps](https://www.npmjs.com/package/@modelcontextprotocol/ext-apps) - Official SDK for building MCP Apps
- [MCP-UI Playground](https://mcpui.dev/) - Interactive playground for testing MCP Apps

### Templates & Boilerplates

- React, Vue, Svelte, and other framework templates in the [official repo](https://github.com/modelcontextprotocol/ext-apps/tree/main/examples)

## Tutorials & Guides

### Getting Started

- [Official MCP Apps Guide](https://modelcontextprotocol.io/docs/extensions/apps) - Complete setup and development guide
- [MCP Apps Interactive UI Guide](https://deepwiki.com/FlorianBruniaux/claude-code-ultimate-guide/6.6-mcp-apps-interactive-ui) - Comprehensive tutorial

### Architecture & Best Practices

- [MCP Apps: The New Standard for Interactive AI Interfaces](https://www.unite.ai/what-are-mcp-apps-the-new-standard-turning-ai-responses-into-interactive-interfaces/) - Overview article
- [MCP Apps are here: Rendering interactive UIs in AI clients](https://workos.com/blog/2026-01-27-mcp-apps) - Technical deep dive

## Use Cases

### Data Exploration
Interactive dashboards where users can filter, sort, and drill down into data without repeated text commands.

**Example**: Sales analysis tool with interactive charts showing revenue by region, with drill-down to account details.

### Configuration Wizards
Multi-step forms with conditional fields that adapt based on user selections.

**Example**: Deployment configuration tool that shows different security options for production vs. staging environments.

### Document Review
Inline document viewers with annotation and approval capabilities.

**Example**: Contract analysis tool displaying PDFs with highlighted key terms, allowing direct approval or flagging.

### Real-time Monitoring
Live dashboards that update automatically without requiring new prompts.

**Example**: Server health monitoring showing real-time metrics, logs, and alerts.

### Interactive Forms
Complex data entry with validation, autocomplete, and dynamic field updates.

**Example**: Multi-step survey or application form with conditional logic.

### Educational Tools
Interactive tutorials, quizzes, and learning experiences.

**Example**: Code playground for learning programming with live execution.

## Contributing

Contributions are welcome! Please read the [contributing guidelines](CONTRIBUTING.md) first.

### How to Contribute

1. **Add a new MCP App**: Submit a PR with the app details in the appropriate category
2. **Improve documentation**: Help make the guides clearer and more comprehensive
3. **Share use cases**: Describe interesting ways you're using MCP Apps
4. **Report issues**: Let us know about broken links or outdated information

### Submission Guidelines

When adding a new MCP App, please include:
- App name with link to repository or website
- Brief description (1-2 sentences)
- Key features or use cases
- Screenshot or demo (if available)
- License information

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.
