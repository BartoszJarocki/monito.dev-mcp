# Directory notes

Which file each MCP directory or marketplace reads. All listings point to the hosted endpoint `https://www.monito.dev/api/mcp`.

| File | Used by | Notes |
| --- | --- | --- |
| `server.json` | Official MCP Registry (`registry.modelcontextprotocol.io`), and the directories that mirror it (GitHub MCP Registry, VS Code MCP gallery, PulseMCP, Glama and others) | Publish with `mcp-publisher publish`, using the `dev.monito` domain proof. The version must be higher than the one already published. |
| `.cursor-plugin/plugin.json`, `mcp.json`, `assets/logo.svg` | Cursor Marketplace | Single-plugin layout at the repository root. Submit the repository link to the Cursor team. |
| `gemini-extension.json` | Gemini CLI extensions gallery (geminicli.com/extensions) | The gallery indexes public GitHub repositories with a `gemini-extension.json` in the root that carry the `gemini-cli-extension` topic. |
| `README.md` | Glama, mcp.so, Smithery listings, awesome-mcp-servers lists, and any directory that asks for a source or documentation link | Install instructions per client, tools, auth and pricing. |
| `LICENSE` | Directories that show or filter by license | MIT covers the files in this repository only, not the hosted service. |
| `assets/logo.png` (512x512) | Directories that ask for a square logo upload | Same icon as `https://www.monito.dev/brand/monito-app-icon-512.png`. |

The repository holds config and docs only. Lists that require the server's source code (for example ToolHive's registry criteria) still do not apply.
