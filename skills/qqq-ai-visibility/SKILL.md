---
name: qqq-ai-visibility
description: Check whether a website is visible to AI search (ChatGPT, Gemini, Perplexity, Claude) with the QQQ AI Visibility Checker MCP server. Use when the user asks if AI crawlers can read a site, how to improve GEO/AEO or AI search readiness, or whether AI assistants mention and cite a brand.
---

# QQQ AI Visibility Checker

The `qqq-ai-visibility` MCP server (https://qqq.ee/mcp) is free, read-only and needs no API key.

- `check_ai_visibility(domain, language)`: technical readiness for AI search. Reports robots.txt and access for AI crawlers (GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot, Google-Extended), sitemap, title, description, H1, canonical, noindex, Organization or Person structured data, JavaScript dependence, key pages and llms.txt, with the three most important fixes.
- `get_public_report(domain, language)`: the domain's latest public report of brand mentions and website citations in ChatGPT, Gemini and Perplexity answers, with the sources the answers used.
- `get_tool_info(language)`: what the checker measures and its limits.

How to use it:

1. For "is my site ready for AI search" questions, call `check_ai_visibility` with the bare domain (example.com).
2. For "do AI assistants mention or cite us" questions, call `get_public_report`. If there is no report yet, pass on the link to the full check.
3. Show the user the qqq.ee link from each result; the full check of live AI answers runs on qqq.ee.
4. `check_ai_visibility` allows five checks per IP address a day. Do not repeat it for the same domain in one conversation.
5. `language` is `en` (default) or `et` (Estonian).
