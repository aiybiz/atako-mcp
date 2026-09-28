# Atako MCP server

[Atako](https://atako.ai) is a platform of AI agents for companies. Its remote MCP server lets an MCP client (Claude Code, Claude Desktop, Cursor, ChatGPT developer mode…) read and act on an Atako account: agents, chat, files, integrations, projects, organization, billing state.

Registry entry: [`ai.atako/mcp`](https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.atako/mcp) (JSON) — manifest in [`server.json`](server.json).
Full documentation: [docs.atako.ai](https://docs.atako.ai/developers/mcp/overview).

This repository holds the registry manifest and connection instructions only. The server is hosted by Atako; there is nothing to install or run.

## Endpoints

Both use the Streamable HTTP transport.

| URL | Auth | Tools |
|---|---|---|
| `https://api.atako.ai/mcp` | `Authorization: Bearer aik_...` (Atako API key) | 86 |
| `https://api.atako.ai/mcp/public` | none | 3: `get_pricing`, `list_use_cases`, `list_integration_pages` |

## API key

[app.atako.ai](https://app.atako.ai) → **Settings → API keys** → **Create key** (company admins). The key is shown once.

| Scope | Over MCP |
|---|---|
| Read | Read-only tools only; write tools are neither listed nor callable. |
| Read & write | All tools, with the permissions of the key's owner. |

Billing and subscription changes are not exposed by any tool. OAuth integrations return a consent URL to open in a browser.

## Toolsets

Restrict the served tools with `?toolsets=` (comma-separated). Without it, all toolsets are served.

```
https://api.atako.ai/mcp?toolsets=agents,messages,projects
```

| Toolset | Tools | Covers |
|---|---|---|
| `agents` | 14 | Agents (create, profile, lifecycle), notices |
| `messages` | 11 | Chat, conversations, cron jobs, sub-agents, activity |
| `files` | 5 | Context files, agent file grants |
| `integrations` | 7 | Integration catalog, connections, grants |
| `billing` | 4 | Credits, subscription, models, harnesses |
| `projects` | 22 | Projects, kanban, card comments, wiki |
| `organization` | 5 | Teams, org chart, members, search |
| `dashboard` | 5 | Company dashboard, credit usage, integration logs (admins) |
| `webhooks` | 6 | Agents' inbound webhooks |
| `email` | 5 | Agents' email addresses, threads, approvals |
| `notifications` | 2 | Your notifications |

Tool list: [docs.atako.ai/developers/mcp/overview](https://docs.atako.ai/developers/mcp/overview#tools).

## Client configuration

### Claude Code

```bash
claude mcp add --transport http atako https://api.atako.ai/mcp \
  --header "Authorization: Bearer aik_..."
```

### Claude Desktop, Cursor

Claude Desktop: **Settings → Developer → Edit Config**. Cursor: `~/.cursor/mcp.json` or `.cursor/mcp.json`.

```json
{
  "mcpServers": {
    "atako": {
      "url": "https://api.atako.ai/mcp",
      "headers": {
        "Authorization": "Bearer aik_..."
      }
    }
  }
}
```

Public endpoint: same config with `https://api.atako.ai/mcp/public` and no `headers`.

## License

[MIT](LICENSE) for the files in this repository. The Atako service is proprietary.
