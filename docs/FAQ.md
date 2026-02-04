# MCP Apps FAQ

Frequently asked questions about MCP Apps.

## General Questions

### What are MCP Apps?

MCP Apps are a standardized extension to the Model Context Protocol that allows AI assistants to display interactive user interfaces directly within conversations, instead of just text responses. They enable rich experiences like dashboards, forms, visualizations, and more.

### How are MCP Apps different from regular MCP tools?

Regular MCP tools return text or JSON data. MCP Apps can return interactive UIs that users can interact with in real-time, without needing to send multiple text prompts back and forth.

### Which AI assistants support MCP Apps?

MCP Apps support is growing. Check with your specific AI assistant provider:
- Claude Desktop (support added in recent versions)
- Custom MCP hosts
- Any host implementing the MCP Apps specification

### Are MCP Apps the same as MCP-UI or OpenAI Apps?

MCP Apps were inspired by both MCP-UI and OpenAI's Apps SDK. The specification standardizes and unifies these approaches into an open standard that works across different AI platforms.

## Technical Questions

### What technologies do I need to know?

- **Frontend**: HTML, CSS, JavaScript (any framework works: React, Vue, Svelte, etc.)
- **Backend**: Any language that can implement MCP (Node.js, Python, etc.)
- **Protocols**: JSON-RPC, HTTP/WebSockets (handled by the SDK)

### Can I use my favorite JavaScript framework?

Yes! MCP Apps work with any frontend framework. The official examples include:
- Vanilla JavaScript
- React
- Vue
- Svelte
- Preact
- Solid

### How is communication handled between the UI and the AI?

Communication uses JSON-RPC over `postMessage` API. The `@modelcontextprotocol/ext-apps` SDK handles this for you:

```javascript
import { App } from "@modelcontextprotocol/ext-apps";
const app = new App();
await app.connect();

// Send message to server
await app.callServerTool({ name: "my_tool", arguments: {} });

// Receive messages from server
app.ontoolresult = (result) => {
  console.log("Received:", result);
};
```

### Is the UI code sandboxed?

Yes! MCP Apps run in sandboxed iframes, providing isolation from the host application for security.

### Can MCP Apps access external APIs?

Yes, but with limitations:
- Your UI JavaScript can make fetch requests to public APIs
- Cross-origin requests must follow CORS policies
- For sensitive operations, use server-side tools instead

### How do I handle state in my MCP App?

You can manage state in multiple ways:

1. **Client-side state**: Use JavaScript variables or state management libraries
2. **Server-side state**: Store state in your MCP server
3. **Hybrid approach**: Store critical state server-side, UI state client-side

```javascript
// Client-side state
let currentFilter = 'all';

// Sync with server when needed
app.callServerTool({
  name: 'save_filter',
  arguments: { filter: currentFilter }
});
```

## Development Questions

### How do I debug my MCP App?

1. **Browser DevTools**: Right-click the app UI and select "Inspect"
2. **Console logging**: Use `console.log()` in your JavaScript
3. **Server logs**: Check your MCP server output
4. **Network tab**: Monitor JSON-RPC messages

### Can I use TypeScript?

Yes! The SDK is written in TypeScript and provides full type definitions:

```typescript
import { App } from "@modelcontextprotocol/ext-apps";
import type { ToolResult } from "@modelcontextprotocol/ext-apps";

const app = new App();

app.ontoolresult = (result: ToolResult) => {
  // Full type safety!
  console.log(result.data);
};
```

### How do I test my MCP App?

1. **Unit tests**: Test your JavaScript logic separately
2. **Integration tests**: Use the `basic-host` example
3. **Manual testing**: Test with a real MCP host like Claude Desktop
4. **Automated testing**: Use Playwright or similar tools

### Can I use CSS frameworks like Tailwind?

Yes! You can include any CSS framework:

```html
<head>
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
```

Or bundle your CSS with your build tool.

### How do I handle large applications?

For complex apps:
1. **Code splitting**: Load modules on demand
2. **Build tools**: Use Vite, webpack, or similar
3. **Optimize bundles**: Minimize and compress
4. **Lazy loading**: Load resources as needed

## Security Questions

### Are MCP Apps secure?

MCP Apps include several security measures:
- **Sandboxed iframes**: Isolate app code from the host
- **JSON-RPC validation**: All messages are validated
- **CORS policies**: External requests follow security rules

However, you should still:
- Validate all inputs
- Sanitize user data
- Never expose secrets in client code

### Can a malicious MCP App access my data?

MCP Apps can only:
- Access their own iframe context
- Call tools you explicitly define
- See data you send them

They cannot:
- Access other apps or conversations
- Read files on your computer (without explicit tool permissions)
- Make unauthorized server requests

### How do I handle authentication?

Authentication should happen on the server side:

```javascript
// In your MCP server
server.setRequestHandler("tools/call", async (request) => {
  // Validate authentication
  const authToken = await validateUser();
  if (!authToken) {
    throw new Error("Unauthorized");
  }
  
  // Process request
  return processToolCall(request);
});
```

Never send authentication tokens or secrets to the client UI.

## Deployment Questions

### How do I deploy an MCP App?

MCP Apps are deployed as part of MCP servers:

1. Package your MCP server (Node.js, Python, etc.)
2. Deploy to your preferred hosting (local, cloud, etc.)
3. Users connect their AI assistant to your server

### Can I host the UI separately from the server?

The UI is served by the MCP server as a resource. You could:
- Serve HTML directly from the server
- Serve a URL that loads external resources
- Use a CDN for static assets

### Do I need a web server?

No! MCP Apps are embedded directly in the AI assistant. You only need:
- An MCP server (can be local)
- The UI resources served by that server

### Can users install my MCP App?

Users install your MCP server (which includes the app) by adding it to their MCP configuration:

```json
{
  "mcpServers": {
    "my-app": {
      "command": "node",
      "args": ["path/to/server.js"]
    }
  }
}
```

## Use Case Questions

### What are MCP Apps good for?

MCP Apps excel at:
- **Data visualization**: Charts, graphs, dashboards
- **Complex forms**: Multi-step wizards, validation
- **Document review**: PDFs, contracts with annotations
- **Real-time monitoring**: Live dashboards, metrics
- **Interactive tools**: Calculators, configurators
- **Media players**: Video, audio, presentations

### What should I NOT use MCP Apps for?

MCP Apps are not ideal for:
- Simple text responses (use regular tools)
- File downloads (use resources)
- Long-running background tasks (use tools with callbacks)
- Standalone web applications (use regular web hosting)

### Can I build a complete application as an MCP App?

You can build complex applications, but consider:
- MCP Apps are best for conversation-embedded experiences
- They work within the constraints of the AI assistant UI
- For full applications, consider a traditional web app with MCP integration

## Community Questions

### How can I contribute to the MCP Apps ecosystem?

1. **Build apps**: Create and share new MCP Apps
2. **Write tutorials**: Help others learn
3. **Report issues**: Improve the specification
4. **Add to awesome list**: Submit your apps to this repository

### Where can I get help?

- [Official MCP Discord/Forum](https://modelcontextprotocol.io/)
- [GitHub Discussions](https://github.com/modelcontextprotocol/ext-apps/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/mcp-apps)
- This repository's [Issues](https://github.com/zeyutt/awsome-mcp-apps/issues)

### Can I use MCP Apps commercially?

Yes! The specification is open and the SDK is licensed permissively. Check the specific licenses:
- MCP specification: Open standard
- SDK: Check the repository license
- Your app: You own the rights

### How can I stay updated?

- Watch the [official repository](https://github.com/modelcontextprotocol/ext-apps)
- Follow MCP Apps announcements
- Join community discussions
- Star this awesome list for updates

## Performance Questions

### How fast are MCP Apps?

Performance depends on:
- Your UI code complexity
- Network latency (for external resources)
- Data processing in your server

Tips for good performance:
- Keep bundles small (< 100KB when possible)
- Use efficient rendering (virtual scrolling, etc.)
- Cache static resources
- Minimize server round trips

### Can I use WebAssembly?

Yes! WebAssembly works in MCP Apps:

```javascript
import init, { my_function } from './my_wasm.js';

await init();
const result = my_function();
```

### How do I optimize load time?

1. **Minimize bundle size**: Remove unused code
2. **Use CDNs**: For common libraries
3. **Lazy load**: Load features on demand
4. **Compress**: Use gzip/brotli
5. **Cache**: Browser caching for static assets

## Advanced Questions

### Can MCP Apps communicate with each other?

Not directly. Each app runs in isolation. However, you can:
- Share data through the server
- Use the model context as a shared state
- Design complementary tools

### Can I build MCP Apps in languages other than JavaScript?

The UI must be HTML/JavaScript (browser requirement). However:
- Your MCP server can be any language (Python, Go, Rust, etc.)
- You can compile to JavaScript (TypeScript, ReScript, Elm, etc.)
- You can use WebAssembly for performance-critical code

### How do I handle real-time updates?

MCP Apps don't have native push notifications, but you can:

1. **Polling**: Periodically fetch updates
```javascript
setInterval(async () => {
  const updates = await app.callServerTool({
    name: 'get_updates'
  });
  render(updates);
}, 5000);
```

2. **WebSocket from server**: If your server maintains connections
3. **Server-sent events**: For one-way streaming

### Can I use a database?

Your MCP server can use any database:
- SQL (PostgreSQL, MySQL, SQLite)
- NoSQL (MongoDB, Redis)
- Cloud databases (Firebase, Supabase)

The UI never directly accesses databases—always go through your server for security.

## Troubleshooting

### My app isn't rendering

Check:
- ✓ Tool has `_meta.ui.resourceUri` defined
- ✓ Resource URI matches server response
- ✓ HTML is valid and complete
- ✓ No JavaScript errors in console

### Communication isn't working

Check:
- ✓ Called `await app.connect()` before other operations
- ✓ Messages are valid JSON
- ✓ Server is running and responding
- ✓ No CORS errors

### Styles not loading

Check:
- ✓ CSS is inline or loaded from accessible URL
- ✓ No Content Security Policy blocking
- ✓ Paths are correct (relative/absolute)

### App is slow

Check:
- ✓ Bundle size (use source maps to analyze)
- ✓ Number of DOM elements (use virtual scrolling)
- ✓ Network requests (minimize round trips)
- ✓ Render efficiency (use React DevTools, etc.)

---

Have a question not answered here? [Open an issue](https://github.com/zeyutt/awsome-mcp-apps/issues) or contribute to this FAQ!
