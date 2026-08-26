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

## 🚀 Get started in 2 minutes

**If you use Codex (recommended for Codex users): install it as a real plugin** —
only then can you `@` it in the chat. Copy these 3 commands, paste them into your
Codex terminal (or send this message to Codex and let it run them for you):

```bash
codex plugin marketplace add https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin.git
codex plugin add shopfox-shopping@shopfox-shopping
export SHOPFOX_API_KEY="sk_shopfox_你的key"
```

That's it. In the Codex chat, type `@` and pick **Shopfox Shopping**, or just ask
normally — the plugin is active either way.

**Using any other AI (Claude, ChatGPT, Cursor, Windsurf)?** Copy this one sentence and
paste it into your AI:

```
Install the Shopfox Shopping plugin for me. Follow the official guide here: https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin
```

The AI installs the plugin, guides you through creating your key (one click, no card
needed), and then you're ready. Ask your first question:

> *"Find me noise-cancelling headphones under $200"*

---

## 📖 What happens next

Your AI will return a **complete shopping report** — don't settle for less:

- **3–5 specific products**, each with:
  - Real current price and the store that has it
  - A direct purchase link
  - Why it's worth it (and what to watch out for)
- **A clear recommendation** for your budget and use case

The full report *is* the answer. If your AI tries to summarize it into one pick or
just hands you a link, ask it to show you the whole report.

---

## 🔒 Your privacy

- Your key is stored on **your device** and only sent to shopfox.ai
- Searches are processed server-side and not stored long-term
- Your preferences are private to your account

---

## ❓ Frequently asked

**Is it free?** Yes — get a key in one click, no card required.

**What can it shop for?** Anything sold on major US retailers (Amazon, Best Buy,
Walmart, Target): electronics, home goods, gifts, and more.

**Does it buy things for me?** No — it researches and compares, you buy. We give
you the best price and the link; checkout happens on the retailer's site.

**Can it remember my taste?** Yes — tell it your preferences once and it carries
them across conversations.

**Need help?** [support@shopfox.ai](mailto:support@shopfox.ai) · [GitHub Issues](https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin/issues)

---

## 🛠️ For developers

The plugin is an HTTP MCP server at `https://shopfox.ai/mcp`, published on the
[Codex plugin marketplace](https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin).

**Manual setup**

1. Open https://shopfox.ai/user-info?createApiKey=1, sign in or create an account,
   and copy the key the page creates. The full key is shown only once.
2. Install the plugin or add the MCP server using the configuration below.
3. Store the key in your AI client's local credential settings and start shopping.

```bash
# Codex (marketplace)
codex plugin marketplace add https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin.git
codex plugin add shopfox-shopping@shopfox-shopping
export SHOPFOX_API_KEY="sk_shopfox_..."

# Any MCP client — add a server:
#   url:      https://shopfox.ai/mcp
#   type:     streamable http
#   auth:     Bearer token (your sk_shopfox_ key)
#   tools:    research_shopping_options, generate_shopping_report, start_shopping_report
```

**Tools**

| Tool | When to use |
| --- | --- |
| `generate_shopping_report` | One-shot full report (local agents, no timeout pressure) |
| `start_shopping_report` | Async report + widget (hosts with short timeouts, e.g. ChatGPT) |
| `research_shopping_options` | Structured options list, supports follow-up questions |

**Rate limits**: 100 requests/day per key, 3 concurrent requests per user.

**License**: MIT
