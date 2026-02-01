# Getting Started with MCP Apps

This guide will help you understand and build MCP Apps from scratch.

## Prerequisites

- Node.js 18+ or Python 3.10+
- Basic understanding of web development (HTML, CSS, JavaScript)
- Familiarity with REST APIs and async programming

## What You'll Learn

1. Understanding MCP Apps architecture
2. Setting up your development environment
3. Creating your first MCP App
4. Deploying and testing your app
5. Best practices and common patterns

## Understanding MCP Apps

### The Big Picture

MCP Apps extend the Model Context Protocol to enable interactive UIs within AI conversations. Instead of:

```
User: "Show me sales data"
AI: "Here's the data in text format: Q1: $100k, Q2: $150k..."
```

You can have:

```
User: "Show me sales data"
AI: [Renders interactive dashboard with charts, filters, and drill-down]
```

### Core Concepts

#### 1. UI Resources

UI resources are HTML/JavaScript bundles served via the `ui://` protocol:

```javascript
// In your MCP server
server.setResourceHandler("ui://charts/sales", () => {
  return {
    type: "text/html",
    content: "<html>... your interactive UI ...</html>"
  };
});
```

#### 2. Tool Metadata

Tools declare which UI resource to use:

```json
{
  "name": "show_sales",
  "description": "Display sales dashboard",
  "inputSchema": { /* ... */ },
  "_meta": {
    "ui": {
      "resourceUri": "ui://charts/sales"
    }
  }
}
```

#### 3. App Class

The `App` class handles communication between your UI and the host:

```javascript
import { App } from "@modelcontextprotocol/ext-apps";

const app = new App();
await app.connect();

// Receive data from the host
app.ontoolresult = (result) => {
  renderChart(result.data);
};

// Call server tools from the UI
const response = await app.callServerTool({
  name: "fetch_details",
  arguments: { id: "123" }
});

// Update conversation context
await app.updateModelContext({
  content: [{ 
    type: "text", 
    text: "User selected option B" 
  }]
});
```

## Quick Start

### 1. Install the SDK

```bash
npm install @modelcontextprotocol/ext-apps
```

### 2. Clone Examples

```bash
git clone https://github.com/modelcontextprotocol/ext-apps
cd ext-apps/examples
```

### 3. Run a Basic Example

```bash
cd basic-server-react
npm install
npm start
```

This will start:
- MCP server on port 3001
- Development server with hot reload

### 4. Test with a Host

You need an MCP-compatible host that supports Apps. Options include:
- Claude Desktop (with MCP Apps support)
- Custom host using the `basic-host` example
- MCP-UI playground at https://mcpui.dev/

## Building Your First App

Let's build a simple "Task Manager" MCP App.

### Step 1: Project Setup

```bash
mkdir task-manager-app
cd task-manager-app
npm init -y
npm install @modelcontextprotocol/ext-apps
npm install @modelcontextprotocol/sdk
```

### Step 2: Create the MCP Server

Create `server.js`:

```javascript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server(
  {
    name: "task-manager",
    version: "1.0.0"
  },
  {
    capabilities: {
      tools: {},
      resources: {}
    }
  }
);

// Define the UI resource
server.setRequestHandler("resources/read", async (request) => {
  if (request.params.uri === "ui://tasks/manager") {
    return {
      contents: [{
        uri: "ui://tasks/manager",
        mimeType: "text/html",
        text: `
<!DOCTYPE html>
<html>
<head>
  <title>Task Manager</title>
  <script type="module">
    import { App } from "https://esm.sh/@modelcontextprotocol/ext-apps@1.0.0";
    
    const app = new App();
    await app.connect();
    
    // Handle tool results
    app.ontoolresult = (result) => {
      const tasks = result.data.tasks || [];
      renderTasks(tasks);
    };
    
    function renderTasks(tasks) {
      const list = document.getElementById('tasks');
      list.innerHTML = tasks.map(task => \`
        <div class="task">
          <input type="checkbox" \${task.done ? 'checked' : ''} 
                 onchange="toggleTask('\${task.id}')">
          <span>\${task.title}</span>
        </div>
      \`).join('');
    }
    
    window.toggleTask = async (id) => {
      await app.callServerTool({
        name: "toggle_task",
        arguments: { id }
      });
    };
  </script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    .task { margin: 10px 0; display: flex; align-items: center; }
    .task input { margin-right: 10px; }
  </style>
</head>
<body>
  <h1>My Tasks</h1>
  <div id="tasks"></div>
</body>
</html>
        `
      }]
    };
  }
  throw new Error("Resource not found");
});

// Define tools
server.setRequestHandler("tools/list", async () => {
  return {
    tools: [
      {
        name: "show_tasks",
        description: "Display the task manager",
        inputSchema: {
          type: "object",
          properties: {}
        },
        _meta: {
          ui: {
            resourceUri: "ui://tasks/manager"
          }
        }
      },
      {
        name: "toggle_task",
        description: "Toggle task completion status",
        inputSchema: {
          type: "object",
          properties: {
            id: { type: "string" }
          },
          required: ["id"]
        }
      }
    ]
  };
});

// Handle tool calls
const tasks = [
  { id: "1", title: "Learn MCP Apps", done: false },
  { id: "2", title: "Build awesome app", done: false }
];

server.setRequestHandler("tools/call", async (request) => {
  const { name, arguments: args } = request.params;
  
  if (name === "show_tasks") {
    return {
      content: [{ 
        type: "resource",
        resource: { uri: "ui://tasks/manager" }
      }],
      data: { tasks }
    };
  }
  
  if (name === "toggle_task") {
    const task = tasks.find(t => t.id === args.id);
    if (task) {
      task.done = !task.done;
    }
    return {
      content: [{ 
        type: "text",
        text: `Task ${args.id} toggled`
      }],
      data: { tasks }
    };
  }
  
  throw new Error("Unknown tool");
});

// Start the server
const transport = new StdioServerTransport();
await server.connect(transport);
```

### Step 3: Configure Your MCP Host

Add to your MCP host configuration (e.g., Claude Desktop):

```json
{
  "mcpServers": {
    "task-manager": {
      "command": "node",
      "args": ["/path/to/task-manager-app/server.js"]
    }
  }
}
```

### Step 4: Test Your App

1. Restart your MCP host
2. Ask: "Show me my tasks"
3. The interactive task manager should appear!

## Best Practices

### Security

1. **Always validate inputs** - Never trust data from the UI
2. **Use sandboxing** - MCP Apps run in iframes, but still validate
3. **Limit permissions** - Only request necessary server capabilities
4. **Sanitize HTML** - Prevent XSS attacks

### Performance

1. **Bundle efficiently** - Minimize HTML/JS size
2. **Lazy load** - Load heavy resources on demand
3. **Cache strategically** - Use browser caching for static assets
4. **Optimize rendering** - Use virtual scrolling for large lists

### User Experience

1. **Loading states** - Show spinners during async operations
2. **Error handling** - Display friendly error messages
3. **Responsive design** - Work on different screen sizes
4. **Accessibility** - Support keyboard navigation and screen readers

### Development Workflow

1. **Use TypeScript** - Better type safety and autocomplete
2. **Hot reload** - Fast iteration during development
3. **Unit tests** - Test your logic separately
4. **Integration tests** - Test with a real MCP host

## Common Patterns

### Pattern 1: Data Visualization

```javascript
app.ontoolresult = (result) => {
  const chart = new Chart(ctx, {
    type: 'bar',
    data: result.data
  });
};
```

### Pattern 2: Forms with Validation

```javascript
const form = document.querySelector('form');
form.addEventListener('submit', async (e) => {
  e.preventDefault();
  
  const formData = new FormData(form);
  const data = Object.fromEntries(formData);
  
  // Validate
  if (!data.email.includes('@')) {
    showError('Invalid email');
    return;
  }
  
  // Submit to server
  await app.callServerTool({
    name: 'submit_form',
    arguments: data
  });
});
```

### Pattern 3: Real-time Updates

```javascript
let pollInterval;

app.ontoolresult = (result) => {
  renderData(result.data);
  
  // Poll for updates
  pollInterval = setInterval(async () => {
    const updated = await app.callServerTool({
      name: 'get_updates'
    });
    renderData(updated.data);
  }, 5000);
};

// Cleanup
window.addEventListener('beforeunload', () => {
  clearInterval(pollInterval);
});
```

## Next Steps

1. Explore the [official examples](https://github.com/modelcontextprotocol/ext-apps/tree/main/examples)
2. Read the [full specification](https://github.com/modelcontextprotocol/ext-apps/blob/main/specification/2026-01-26/apps.mdx)
3. Join the community and share your apps
4. Contribute to this awesome list!

## Troubleshooting

### App doesn't render

- Check that the tool has `_meta.ui.resourceUri` set
- Verify the UI resource URI matches what the server returns
- Check browser console for errors

### Communication errors

- Ensure `app.connect()` is called before other operations
- Check that messages are valid JSON-RPC
- Verify the host supports MCP Apps

### Performance issues

- Profile your JavaScript code
- Reduce bundle size
- Use code splitting for large apps

## Resources

- [Official SDK Documentation](https://modelcontextprotocol.github.io/ext-apps/api/)
- [MCP Apps Specification](https://github.com/modelcontextprotocol/ext-apps)
- [Community Examples](https://github.com/zeyutt/awesome-mcp-apps)
- [MCP-UI Playground](https://mcpui.dev/)
