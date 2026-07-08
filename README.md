# BaseThread MCP

The public home for [BaseThread](https://basethread.ai)'s Model Context Protocol server , its registry
manifest, install guide, and security page.

BaseThread is one shared context your whole team's AI tools read and write over MCP. Claude Code, Cursor,
ChatGPT, Windsurf, and Copilot each read the relevant slice of a shared, company-structured context graph
at the start of a session and write activity, decisions, and tasks back as work happens , so no one
re-explains the project and answers stop contradicting. Free to start, no card.

## Connect

The server is hosted (remote, streamable-HTTP + OAuth) , no local install needed:

```bash
claude mcp add --transport http basethread https://mcp.basethread.ai/mcp
```

Or add `https://mcp.basethread.ai/mcp` in any MCP client that supports remote servers. Full instructions
for every client, plus the tool list: **[llms-install.md](./llms-install.md)**.

## In this repo

- **[server.json](./server.json)** , the Official MCP Registry manifest (`ai.basethread/basethread`).
- **[llms-install.md](./llms-install.md)** , agent-readable install + configuration guide.
- **[SECURITY.md](./SECURITY.md)** , security, permissions, auth, and tenancy.
- **[PUBLISHING.md](./PUBLISHING.md)** , how the registry listing is published and republished.

## Links

- Product: https://basethread.ai
- App: https://app.basethread.ai
- Live demo (no signup): https://app.basethread.ai/demo
- MCP endpoint: https://mcp.basethread.ai/mcp
