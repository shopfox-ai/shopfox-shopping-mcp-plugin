# Shopfox Shopping

**Ask your AI to shop for you — get real prices, real stores, real links.**

Shopfox turns any AI assistant into a shopping expert. Ask in plain English, and it
researches live offers across Amazon, Best Buy, Walmart, and Target — then delivers a
complete report: 3–5 top picks with current prices, ratings, pros & cons, and direct
purchase links. No more guessing, no more tab-hopping.

---

## ✨ What you can ask

| You say | You get |
|---|---|
| "Find noise-cancelling headphones under $200" | Top 5 picks, prices, links |
| "Birthday gift for a photographer, budget $200" | Thoughtful picks for their use case |
| "55-inch TV for a bright living room, under $800" | Recommendations matched to your room |
| "Compare gaming laptops around $1000, $1500, $2000" | Side-by-side picks at each budget |
| "I prefer Apple accessories, now find me earbuds under $180" | Recommendations that remember you |

---

## 🚀 Install

### Codex

```bash
codex plugin marketplace add https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin.git
codex plugin add shopfox-shopping@shopfox-shopping
```

Then type `@shopfox` in Codex. The first time you use it, Codex opens a Shopfox
sign-in page in your browser (OAuth) — no keys to copy.

### Claude Code

```bash
claude plugin marketplace add shopfox-ai/shopfox-shopping-mcp-plugin
claude plugin install shopfox-shopping@shopfox
```

Claude Code asks for your Shopfox API key when it enables the plugin. Get one in one
click at https://www.shopfox.ai/user-info?createApiKey=1 (sign in or sign up, the key
is created automatically and shown once).

### Any other MCP client (Cursor, Windsurf, ChatGPT, …)

Add a remote MCP server:

| Setting | Value |
|---|---|
| URL | `https://mcp.shopfox.ai/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth (automatic), or `Authorization: Bearer sk_shopfox_…` with a key from the link above |

Then ask your first question:

> *"Find me noise-cancelling headphones under $200"*

---

## 📖 What happens next

Your AI will return a **complete shopping report** — don't settle for less:

- **3–5 specific products**, each with:
  - Real current price and the store that has it
  - A direct purchase link
  - Why it's worth it (and what to watch out for)
- **A clear recommendation** for your budget and use case

A full report takes a few minutes to research. The full report *is* the answer. If
your AI tries to summarize it into one pick or just hands you a link, ask it to show you
the whole report.

---

## 🔒 Your privacy

- Your sign-in or key is stored on **your device** and only sent to shopfox.ai
- Searches are processed server-side and not stored long-term
- Your preferences are private to your account
- [Privacy policy](https://www.shopfox.ai/privacy-policy) · [Terms of service](https://www.shopfox.ai/terms-of-service)

---

## ❓ Frequently asked

**Is it free?** Yes — sign up in one click, no card required.

**What can it shop for?** Anything sold on major US retailers (Amazon, Best Buy,
Walmart, Target): electronics, home goods, gifts, and more.

**Does it buy things for me?** No — it researches and compares, you buy. We give
you the best price and the link; checkout happens on the retailer's site.

**Can it remember my taste?** Yes — tell it your preferences once and it carries
them across conversations.

**Need help?** [support@shopfox.ai](mailto:support@shopfox.ai) · [GitHub Issues](https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin/issues)

---

## 🛠️ For developers

The plugin is a remote MCP server (Streamable HTTP) at `https://mcp.shopfox.ai/mcp`.

**Layout of this repository**

| Path | Purpose |
|---|---|
| `plugin.json`, `mcp.json` | Portable [Agent Plugins](https://agent-plugins.org) manifest (OpenAI / Codex / ChatGPT), OAuth via dynamic client registration |
| `.codex-plugin/plugin.json` | Compatibility manifest for older Codex builds |
| `.agents/plugins/marketplace.json` | Codex marketplace catalog (install from this repo) |
| `.claude-plugin/plugin.json`, `.mcp.json` | Claude Code plugin manifest; the API key is collected through `userConfig` |
| `.claude-plugin/marketplace.json` | Claude Code marketplace catalog (install from this repo) |
| `skills/shopping-advisor/SKILL.md` | The skill that tells the agent how to use the tools |
| `agents/openai.yaml` | Agent dependency declaration |

**Authentication**

- OAuth 2.1 with PKCE and dynamic client registration. Discovery:
  `https://mcp.shopfox.ai/.well-known/oauth-authorization-server` and
  `https://mcp.shopfox.ai/.well-known/oauth-protected-resource`. Scope: `mcp:shopping`.
- Or a personal API key as a bearer token: `Authorization: Bearer sk_shopfox_…`,
  created at https://www.shopfox.ai/user-info?createApiKey=1 (or **Settings → Agent API**
  in the Shopfox web app).

**Tools**

| Tool | When to use |
| --- | --- |
| `generate_shopping_report` | One-shot full report (local agents, no timeout pressure) |
| `start_shopping_report` | Async report + widget (hosts with short timeouts, e.g. ChatGPT) |
| `research_shopping_options` | Structured options list, supports follow-up questions |

**Rate limits**: 100 requests/day per key, 3 concurrent requests per user.

**Validate locally**

```bash
claude plugin validate --strict .
codex plugin marketplace add "$PWD" && codex plugin add shopfox-shopping@shopfox-shopping
```

**License**: Apache-2.0
