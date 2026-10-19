<p align="center"><img src="icon.png" width="120" height="120" alt="QQQ logo"></p>

# QQQ AI Visibility Checker: MCP server

[![smithery badge](https://smithery.ai/badge/q-y0oi/ai-visibility)](https://smithery.ai/servers/q-y0oi/ai-visibility) [![AllMCPs Verified](https://allmcps.com/api/badge/qqq-ai-visibility-mcp)](https://allmcps.com/mcp/qqq-ai-visibility-mcp?verify=2890c9be-8656-43fd-9734-88e04d412a9b)

Free AI visibility and GEO check for any website, inside Claude, ChatGPT, Cursor, VS Code and other MCP clients. It tells you whether AI crawlers (GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot, Google-Extended and others) may read a site, whether the site is technically ready for AI search, and what the latest public report says about brand mentions and citations in ChatGPT, Gemini and Perplexity answers.

- **Server URL:** `https://qqq.ee/mcp`
- **Transport:** Streamable HTTP (stateless JSON responses)
- **Authentication:** none, no API key
- **Registry name:** `ee.qqq/ai-visibility`
- **Documentation:** https://qqq.ee/en/mcp-server/ (Estonian: https://qqq.ee/mcp-server/)
- **Web tool:** https://qqq.ee/en/ai-visibility-checker/

This repository holds the documentation and client configuration only. The server is hosted by QQQ OÜ on qqq.ee; there is nothing to install or run.

## Tools

| Tool | Input | Returns |
|---|---|---|
| `check_ai_visibility` | `domain` (required), `language`: `en` (default) or `et` | Technical readiness for AI search: homepage, HTTPS, robots.txt, AI crawler access, sitemap, title, description, H1, canonical, noindex, Organization/Person structured data, JavaScript dependence, key pages, llms.txt; the three most important fixes; the latest public report of the domain (mentions out of all answers, citations of the company's own website) when one exists; a prefilled link to the full check on qqq.ee. |
| `get_public_report` | `domain` (required), `language` | The latest public report of a domain, if someone has run the full check on qqq.ee: date, market and field, mentions in search-grounded AI answers, citations of the company's own website, results per AI engine, the questions asked, the most cited sources, other companies named and short answer excerpts, plus the link to the report page. Fetches nothing and asks no AI model. Without a report it says so and links the full check. |
| `get_tool_info` | `language` | What the checker measures, its limits and the English and Estonian links. Fetches nothing. |

All three tools are read-only. The full check of live AI answers (ChatGPT, Gemini, Perplexity) runs on qqq.ee, and every answer from the server links there.

## Connect

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=qqq-ai-visibility&config=eyJ1cmwiOiJodHRwczovL3FxcS5lZS9tY3AifQ==) [![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_MCP_server-0098FF?logo=visualstudiocode&logoColor=white)](https://vscode.dev/redirect/mcp/install?name=qqq-ai-visibility&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fqqq.ee%2Fmcp%22%7D)

### Claude (claude.ai, Claude Desktop)

Settings → Connectors → Add custom connector → URL `https://qqq.ee/mcp`, no authentication.

### Claude Code

```bash
claude mcp add --transport http qqq-ai-visibility https://qqq.ee/mcp
```

### ChatGPT

Developer mode → Settings → Connectors → add a server with URL `https://qqq.ee/mcp`, authentication "No authentication".

### Cursor

`~/.cursor/mcp.json` or `.cursor/mcp.json` in a project ([examples/cursor.mcp.json](examples/cursor.mcp.json)):

```json
{
  "mcpServers": {
    "qqq-ai-visibility": {
      "url": "https://qqq.ee/mcp"
    }
  }
}
```

### VS Code

`.vscode/mcp.json` ([examples/vscode.mcp.json](examples/vscode.mcp.json)):

```json
{
  "servers": {
    "qqq-ai-visibility": {
      "type": "http",
      "url": "https://qqq.ee/mcp"
    }
  }
}
```

### Windsurf

Windsurf MCP config (Cascade → MCP servers → View raw config):

```json
{
  "mcpServers": {
    "qqq-ai-visibility": {
      "serverUrl": "https://qqq.ee/mcp"
    }
  }
}
```

### Gemini CLI

```bash
gemini mcp add --transport http qqq-ai-visibility https://qqq.ee/mcp
```

or `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "qqq-ai-visibility": {
      "httpUrl": "https://qqq.ee/mcp"
    }
  }
}
```

### OpenAI Codex CLI

```bash
codex mcp add qqq-ai-visibility --url https://qqq.ee/mcp
```

or `~/.codex/config.toml`:

```toml
[mcp_servers.qqq-ai-visibility]
url = "https://qqq.ee/mcp"
```

### Clients that only support stdio

Use the `mcp-remote` bridge ([examples/claude-desktop-stdio.json](examples/claude-desktop-stdio.json)):

```json
{
  "mcpServers": {
    "qqq-ai-visibility": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://qqq.ee/mcp"]
    }
  }
}
```

Then ask, for example: *"Check the AI visibility of example.com."*

## Plugins

This repository is also a plugin for agents that install plugins from GitHub. It bundles the remote server config and a skill that tells the agent when to use each tool:

- Claude Code: `/plugin marketplace add meelis545/qqq-ai-visibility-mcp`, then `/plugin install qqq-ai-visibility@qqq-ai-visibility` (`.claude-plugin/`, `.mcp.json`, `skills/`)
- Cursor: `.cursor-plugin/plugin.json` and `mcp.json`
- Agent Plugins (agent-plugins.org): `plugin.json` and `mcp.json`

## Limits

| What | Limit |
|---|---|
| `check_ai_visibility` per IP address | 5 a day (UTC) |
| `check_ai_visibility` in total | 300 a day |
| Invalid domain | does not use up a check |
| `get_public_report`, `get_tool_info`, `initialize`, `tools/list`, `ping` | no limit |
| Request body | 16 KB, at most 10 messages in a batch |

Only public websites are checked: IP addresses, localhost and internal hostnames are refused, and redirects are validated again.

## Privacy

- The IP address is only the key of a daily counter; counters are deleted after two days.
- Usage statistics (90 days): step, language, domain, status, country, duration. No IP address, no client name.
- Headers, request bodies, MCP client details and keys are not logged.
- An MCP check does not create a public report and sends nothing to AI models. `get_public_report` only reads a report that is already public on qqq.ee.

Full policy: https://qqq.ee/en/privacy-policy/

## Protocol details

- Protocol versions: `2025-11-25`, `2025-06-18`, `2025-03-26`
- No `Mcp-Session-Id`, no SSE stream; `GET` from a browser redirects to the web tool, other `GET` requests get `405`
- CORS open (`*`): no cookies or credentials are used
- [`server.json`](server.json) is the entry in the official MCP Registry

## Contact

QQQ OÜ, Niine 11, 10414 Tallinn, Estonia · q@qqq.ee · https://qqq.ee/en/

QQQ OÜ is an SEO and AI search agency and is not affiliated with Invesco QQQ Trust or the Nasdaq ticker QQQ.
