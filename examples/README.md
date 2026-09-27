# Client examples

| File | Client | Where it goes |
| --- | --- | --- |
| `cursor-mcp.json` | Cursor | `~/.cursor/mcp.json` or `.cursor/mcp.json` in a project |
| `vscode-mcp.json` | VS Code (Copilot agent mode) | `.vscode/mcp.json` |
| `workbuddy-mcp.json` | WorkBuddy / 千问办公 / other `mcpServers` clients | client MCP settings |

All of them use OAuth: the first tool call opens the One1 sign-in page. To use an API key instead, add
`"headers": { "X-Api-Key": "YOUR_ONE1_API_KEY" }` (VS Code / Cursor accept the same `headers` field).
