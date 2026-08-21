---
name: shopping-advisor
description: Use the Shopfox shopping agent (via the shopfox MCP server) to research products and produce purchase reports for US shoppers. Call generate_shopping_report for a one-shot report, research_shopping_options for structured research with clarification, or start_shopping_report on hosts with a short timeout (ChatGPT).
license: Apache-2.0
metadata:
  allowed_mcp_servers:
    - shopfox
---

# Shopfox Shopping Advisor

This skill connects you to the Shopfox shopping agent through the `shopfox` MCP server. All the real work — current-market verification, retailer search, offer lookup, and the purchase report — happens server-side. Your job is only to call the right tool with the right input and relay the result.

## Tools

### `generate_shopping_report` — one-shot report (preferred)

Use this when the user wants a complete recommendation report from a single request.

Input:
- `query` (required) — what to research, e.g. "best 55-inch TV under $800 for a bright living room".
- `context` (optional) — extra constraints such as budget, brands, use case, size, or platform.
- `language` (optional) — report language ("en", "zh"); omit to auto-detect from the query.

Returns the full report as markdown plus a structured product summary. Call it once and relay the markdown to the user.

### `research_shopping_options` — structured research

Use this when you want to drive the research yourself and get structured data back.

Input:
- `query` (required)
- `budget_min` / `budget_max` (optional, USD)
- `must_have` / `avoid` (optional arrays of hard requirements / exclusions)
- `clarification_answers` (optional — answers to the questions returned in a previous `needsClarification` result)

Returns markdown plus `structuredContent`. If the request is too vague it returns `needsClarification` with structured questions: ask the user, then call the tool again with `clarification_answers`.

### `start_shopping_report` — async start (ChatGPT / short-timeout hosts)

Use this when the host times out around 60 seconds. It returns immediately with `jobId`, `statusUrl`, and `widgetUrl`. Poll `statusUrl` until `status` is `done`, then relay `markdown`.

## Rules

- US shoppers only; all prices and budgets in USD. Do not introduce Chinese domestic platforms (JD, Taobao, Tmall, Pinduoduo, Douyin, Xiaohongshu Shop).
- Do not invent prices, sellers, links, stock, or ratings — only relay what the tool returns.
- A report with no products means the agent found no verifiable offers; say so plainly, never fabricate.
- Prefer `generate_shopping_report` unless you specifically need the structured `needsClarification` flow.
- On ChatGPT or any host with a short request timeout, use `start_shopping_report` instead of waiting on `generate_shopping_report`.
