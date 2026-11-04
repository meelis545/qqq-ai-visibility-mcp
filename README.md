<p align="center"><img src="icon.png" width="120" height="120" alt="QQQ logo"></p>

# QQQ AI Visibility Checker: MCP server

[![LightNow capabilities](https://lightnow.ai/badge/ee.qqq/ai-visibility)](https://lightnow.ai/servers/ee.qqq/ai-visibility) [![AllMCPs Verified](https://allmcps.com/api/badge/qqq-ai-visibility-mcp)](https://allmcps.com/mcp/qqq-ai-visibility-mcp?verify=2890c9be-8656-43fd-9734-88e04d412a9b)

Free AI visibility and GEO check for any website, inside Claude, ChatGPT, Cursor, VS Code and other MCP clients. It tells you whether AI crawlers (GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot, Google-Extended and others) may read a site, whether the site is technically ready for AI search, and what the latest public report says about brand mentions and citations in answers from ChatGPT, Gemini, Perplexity, Claude and Kimi.

- **Server URL:** `https://qqq.ee/mcp` (use this exact host: `www.qqq.ee` redirects, and a redirect breaks POST requests)
- **Transport:** Streamable HTTP (stateless JSON responses)
- **Authentication:** none, no API key
- **Registry name:** `ee.qqq/ai-visibility`
- **Documentation:** https://qqq.ee/en/mcp-server/ (Estonian: https://qqq.ee/mcp-server/)
- **Web tool:** https://qqq.ee/en/ai-visibility-checker/
- **Changes:** [CHANGELOG.md](CHANGELOG.md)

This repository holds the documentation and client configuration only. The server is hosted by QQQ OÜ on qqq.ee; there is nothing to install or run.

## Tools

| Tool | Input | Returns |
|---|---|---|
| `check_ai_visibility` | `domain` (required), `language`: `en` (default) or `et` | Technical readiness for AI search: homepage, HTTPS, robots.txt, AI crawler access, sitemap, title, description, H1, canonical, noindex, Googlebot access, Organization/Person structured data, JavaScript dependence, key pages, llms.txt and whether the server really answers AI crawlers (the last two are informational); the three most important fixes; the latest public report of the domain (mentions out of all answers, citations of the company's own website) when one exists; a prefilled link to the full check on qqq.ee. |
| `get_public_report` | `domain` (required), `language` | The latest public report of a domain, if someone has run the full check on qqq.ee: date, market and field or buyer phrase, mentions in search-grounded AI answers, citations of the company's own website, results per AI engine, the questions asked, the most cited sources by kind, other companies named and short answer excerpts, plus the link to the report page ([what a report contains](#what-a-public-report-contains)). Fetches nothing and asks no AI model. Without a report it says so and links the full check. |
| `get_tool_info` | `language` | What the checker measures, its limits and the English and Estonian links. Fetches nothing. |

All three tools are read-only. The full check of live AI answers runs on qqq.ee, and every answer from the server links there.

## What a public report contains

`get_public_report` returns readable text and the same data as `structuredContent.report`. Reports are stored as they were measured, so an older report has fewer fields: a missing field means the report predates it, not zero.

| Field | Meaning |
|---|---|
| `mentions` | Of the answers that searched the web: how many named the company, how many recommended it, and how many cited its website without naming it (`citedNotNamed`). |
| `citations` | How many answers cited the company's own website. `consultedNotCited` counts answers whose search found the website but did not use it, only for engines that show every page they looked at. |
| `engines`, `answers` | Results per engine and short excerpts: Gemini, Perplexity, Claude, Kimi and ChatGPT through their APIs, and Llama from model memory, which is a separate comparison. `outcome` says whether an answer recommended, named or cited the company, or did neither. |
| `chatgptWeb` | The same buyer questions asked on chatgpt.com: answers that named the company, answers that cited its website, answers received, answers still pending. Reported apart and never added to `mentions` or `citations`. |
| `questions` | The buyer questions asked. `ownPage` says whether the company's own page was a source (`cited`), was found but not used (`consulted`), was not found (`none`), or cannot be told (`unknown`). |
| `sources` | The most cited domains. `kind` and `byKind` sort them into own website, media, directory, community, public sector and other. |
| `topic` | The buyer phrase the questions were built from, with its monthly Google search volume in Estonia. |
| `google` | For the field's buyer query: whether Google showed an AI Overview, whether the company's website was among its references, and its organic top 10 position. Not an AI answer. |
| `competitorsYours` | When the person running the check named their own competitors: counts only. The names stay in that person's private link and never appear in a public report or through this server. |

There is no combined score: mentions, citations and technical readiness stay separate.

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
- OpenClaw / ClawHub: `clawhub/qqq-ai-visibility/SKILL.md`, a skill that calls the same tools with `curl` (no MCP client needed)

## Limits

| What | Limit |
|---|---|
| `check_ai_visibility` per IP address (per calling site for Cloudflare Workers) | 5 a day (UTC) |
| `check_ai_visibility` in total | 300 a day |
| Invalid domain | does not use up a check |
| `get_public_report`, `get_tool_info`, `initialize`, `tools/list`, `ping` | no limit |
| Request body | 16 KB, at most 10 messages in a batch |

Only public websites are checked: IP addresses, localhost and internal hostnames are refused, and redirects are validated again.

## Privacy

- The IP address is only the key of a daily counter; counters are deleted after two days. Calls from Cloudflare Workers all share one Cloudflare address, so for them the key is the calling site's domain, which Cloudflare adds to the request.
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
