# Changelog

Changes to the hosted server at `https://qqq.ee/mcp`, newest first. There is nothing to install, so every client gets a change as soon as it is live.

The server still reports version 1.1.0. The tools and their inputs have not changed since that version; everything below adds to what the tools return or changes how limits are counted.

## 2026-11-04

- Reports made from this date on include `chatgptWeb`: the same buyer questions asked on chatgpt.com, with the answers that named the company and the answers that cited its website. The counts are reported apart from the API answers and are not added to `mentions` or `citations`.
- The text of `get_public_report` has a matching line.

## 2026-11-01

- `check_ai_visibility`: calls from Cloudflare Workers all share one Cloudflare address, so their daily limit of five checks is now counted per calling site instead of being shared by everyone.

## 2026-10-26

- `get_public_report`: each question has `ownPage`, which says whether the company's own page was a source, was found but not used, was not found, or cannot be told.
- `get_public_report`: cited domains have a `kind` (own website, media, directory, community, public sector, other), with totals in `sources.byKind`.
- `get_public_report`: `competitorsYours` gives counts when the person running the check named their own competitors. The names are never returned.
- `get_public_report`: `topic` gives the buyer phrase the questions were built from and its monthly search volume, for reports that used one.

## 2026-10-21

- `check_ai_visibility`: a new informational check says whether the server really answers AI crawlers, beyond what robots.txt allows.
- An older report without counts now shows its date and link instead of "0 of 0".

## 2026-10-18

- `get_public_report`: `google` reports whether Google showed an AI Overview for the field's buyer query, whether the company's website was among its references, and its organic position. With DataForSEO the query is fetched three times, so the result reads "k of 3".
- `get_public_report`: `citations.consultedNotCited` counts answers whose search found the website but did not use it.
- `get_public_report`: `mentions.citedNotNamed` counts answers that cited the website without naming the company, and each answer has an `outcome`.

## 1.1.0 (2026-10-17)

- New tool `get_public_report`: the latest public report of a domain as data. It fetches nothing and has no daily limit.

## 1.0.0 (2026-10-17)

- First version in the official MCP Registry as `ee.qqq/ai-visibility`, with the tools `check_ai_visibility` and `get_tool_info`.
