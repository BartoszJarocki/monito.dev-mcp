<p align="center">
  <img src="assets/logo.svg" width="96" height="96" alt="Monito">
</p>

# Monito MCP server

Monito's hosted MCP server lets an agent check a deployed URL in a real browser. You give it a URL and a plain-English expectation. It returns **passed**, **failed** or **cannot verify**, with screenshots, network requests and console logs as evidence. A check can be saved as a reusable test.

This repository holds the connection config and directory metadata for the hosted server. The server itself runs at Monito; there is nothing to build or run from here.

| | |
| --- | --- |
| Endpoint | `https://www.monito.dev/api/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth (browser sign-in), or an API key in the `x-api-key` header |
| Registry name | `dev.monito/monito` |
| Docs | https://www.monito.dev/docs/mcp |

The hosted server tests deployed URLs and rejects `localhost`. To check a local dev server, use the [local server](#local-server).

## Connect

### Claude Code

```bash
claude mcp add --transport http monito https://www.monito.dev/api/mcp
```

Then run `/mcp` and choose **monito** to sign in.

### Claude (web and desktop)

Settings → Connectors → **Add custom connector**, and enter `https://www.monito.dev/api/mcp`.

### Cursor

Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):

```json
{
  "mcpServers": {
    "monito": {
      "url": "https://www.monito.dev/api/mcp"
    }
  }
}
```

Open Cursor Settings → MCP and click **Needs login** next to monito.

### VS Code

```bash
code --add-mcp '{"name":"monito","type":"http","url":"https://www.monito.dev/api/mcp"}'
```

Or add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "monito": {
      "type": "http",
      "url": "https://www.monito.dev/api/mcp"
    }
  }
}
```

### Gemini CLI

```bash
gemini extensions install https://github.com/BartoszJarocki/monito.dev-mcp
```

The extension adds the hosted server (`gemini-extension.json`). Sign in when Gemini CLI prompts, or run `/mcp auth monito`.

### ChatGPT

In developer mode, add a connector with the URL `https://www.monito.dev/api/mcp` and OAuth authentication.

### Other clients

Any MCP client that supports Streamable HTTP can connect to `https://www.monito.dev/api/mcp`. Clients without OAuth support can send an API key (see below).

### Local server

The local server runs from the Monito CLI over stdio. It can check `localhost` through a tunnel, and it is the option for clients that only run local servers, such as Codex:

```bash
npx -y @monitodev/cli mcp
```

```toml
# ~/.codex/config.toml
[mcp_servers.monito]
command = "npx"
args = ["-y", "@monitodev/cli", "mcp"]
```

It uses your `monito auth login` session or the `MONITO_TOKEN` environment variable.

## Authentication

**OAuth (default).** The first connection opens a Monito sign-in page in your browser. Choose **Allow** and the client gets a token for your account. Access tokens expire after an hour and clients renew them for up to 30 days. Disconnect an app at any time in Settings → Connected apps.

**API key.** Clients that cannot open a browser can send a key instead. Create one in Account settings in the Monito dashboard (keys start with `monito_`) and send it as a header:

```text
x-api-key: monito_...
```

For example, in Claude Code:

```bash
claude mcp add --transport http monito https://www.monito.dev/api/mcp \
  --header "x-api-key: $MONITO_API_KEY"
```

A connected app can see and change your projects, scenarios and runs, and can start runs that spend credits. It cannot create API keys, change your plan or manage connected apps. Details: https://www.monito.dev/docs/mcp/authentication

## Tools

The default toolsets are `check` and `core-read` (ten tools):

| Tool | What it does | Uses credits |
| --- | --- | --- |
| `check` | Check a URL against an expectation and wait for the verdict and report link | Yes |
| `save_check` | Save a check as a named, reusable test | |
| `get_run` | Verdict, bug reports, failed requests, console errors and screenshot names | |
| `get_screenshot` | One screenshot from a run, returned as an image | |
| `wait_for_run` | Keep waiting for a run that is still going | |
| `list_projects` | Your projects, optionally filtered by name | |
| `list_scenarios` | Saved scenarios, optionally for one project | |
| `list_runs` | Your recent runs, optionally for one project | |
| `run_scenario` | Run one saved scenario and wait for the verdict | Yes |
| `run_project` | Run a project's saved scenarios, or one suite | Yes |

Add `?toolsets=all` to the endpoint URL for the full catalog (projects and scenarios management, discovery, defects, automations and fixtures). The server also provides a `verify-change` prompt (`/mcp__monito__verify-change` in Claude Code). Full reference: https://www.monito.dev/docs/mcp/tools

## Pricing

Each browser run uses Monito credits. The free plan includes 50 test credits a month. Paid plans: https://www.monito.dev/pricing

## Files in this repository

| File | Purpose |
| --- | --- |
| `server.json` | MCP Registry metadata for `dev.monito/monito` |
| `mcp.json`, `.cursor-plugin/plugin.json` | Cursor plugin |
| `gemini-extension.json` | Gemini CLI extension |
| `assets/logo.svg`, `assets/logo.png` | Monito app icon |

## Support

Questions and problems: open an issue in this repository, or see https://www.monito.dev/docs/mcp.

## License

The files in this repository are available under the [MIT License](LICENSE). The Monito service is subject to its own terms: https://www.monito.dev
