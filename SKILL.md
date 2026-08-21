---
name: shopfox-shopping
description: Use the Shopfox shopping agent (via the shopfox MCP server) to research products and produce purchase reports for US shoppers. Call generate_shopping_report for a one-shot report, research_shopping_options for structured research with clarification, or start_shopping_report on hosts with a short timeout (ChatGPT).
license: Apache-2.0
metadata:
  allowed_mcp_servers:
    - shopfox
---

# Shopfox Shopping Advisor

This skill connects you to the Shopfox shopping agent through the `shopfox` MCP server. All the real work — current-market verification, retailer search, offer lookup, and the purchase report — happens server-side. Your job is only to call the right tool with the right input and relay the result.

## Before You Start: Get Your API Key

The `shopfox` MCP server requires a personal API key (`sk_shopfox_...`). If the user doesn't have one yet, walk them through this — do not guess or skip it:

1. Tell the user to open this link in their browser:
   **https://shopfox.ai/user-info?createApiKey=1**
2. They log in with their Shopfox account (or sign up). A key is created automatically once they're signed in and shown on the page **only once**.
3. Ask the user to copy the key and paste it to you (or you read it back from them), then configure the MCP server entry `shopfox` with `Authorization: Bearer <key>`.
4. If the link didn't create a key (e.g. they were already signed in), they can always generate one manually at **Settings → Agent API** in the Shopfox web app.

Never ask the user to send their key in plaintext chat logs if the host stores them — prefer configuring the MCP server directly and keeping the key out of the conversation transcript.

## Tools

### `generate_shopping_report` — one-shot report (preferred)

Use this when the user wants a complete recommendation report from a single request.

Input:
- `query` (required) — what to research, e.g. "best 55-inch TV under $800 for a bright living room".
- `context` (optional) — extra constraints such as budget, brands, use case, size, or platform.
- `language` (optional) — report language ("en", "zh"); omit to auto-detect from the query.

Returns the **full report as markdown** — 3-5 concrete products with real prices, purchase links, pros/cons, and best use cases.
**Relay the returned markdown to the user VERBATIM.** Do not summarize it, do not shorten it, do not pick a single product, and do not add your own "recommendation" on top. The report is the answer.

### `research_shopping_options` — structured research

Use this when you want to drive the research yourself and get structured data back.

Input:
- `query` (required)
- `budget_min` / `budget_max` (optional, USD)
- `must_have` / `avoid` (optional arrays of hard requirements / exclusions)
- `clarification_answers` (optional — answers to the questions returned in a previous `needsClarification` result)

Returns markdown plus `structuredContent`. If the request is too vague it returns `needsClarification` with structured questions: ask the user, then call the tool again with `clarification_answers`.

### `start_shopping_report` — async start (ChatGPT / short-timeout hosts)

Use this only when the host times out around 60 seconds. It returns immediately with `jobId`, `statusUrl`, and `widgetUrl`.

**You must then poll `statusUrl` yourself (every 3 seconds) until `status` is `done`, and output the complete markdown report directly in your reply.** Never show the status/widget URLs to the user.

## Rules

- **Never give the user a URL or link to a report** (no status URL, no widget URL, no "open this link" messages). The full report must always appear directly in your reply, as markdown.
- **Output the report in full, verbatim.** When a tool returns a complete report (3-5 products with prices, links, pros/cons), relay all of it to the user. Never summarize it into a single pick, never shorten it, never add your own brief "recommendation" instead of the report.
- If you used `start_shopping_report`, poll `statusUrl` yourself until `status` is `done`, then output the full `markdown` report in your reply. Do not ask the user to check a link.
- If you used `research_shopping_options` and got `needsClarification`, ask the user the clarifying questions first, then call the tool again with `clarification_answers` — never fall back to telling the user to open a link.
- US shoppers only; all prices and budgets in USD. Do not introduce Chinese domestic platforms (JD, Taobao, Tmall, Pinduoduo, Douyin, Xiaohongshu Shop).
- Do not invent prices, sellers, links, stock, or ratings — only relay what the tool returns.
- A report with no products means the agent found no verifiable offers; say so plainly, never fabricate.
- Prefer `generate_shopping_report` unless you specifically need the structured `needsClarification` flow.
- On ChatGPT or any host with a short request timeout, use `start_shopping_report` instead of waiting on `generate_shopping_report`.
