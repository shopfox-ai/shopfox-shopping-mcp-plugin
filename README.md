# Shopfox Shopping Agent

> US shopping research and offer comparison for Claude Code, ChatGPT, Cursor, and other AI assistants — one report per request, with real prices and purchase links.

## ⚡ Easiest Way: Let Your Agent Install It

Copy and paste this into **any** AI agent that supports MCP (Claude Code, Cursor, Windsurf, Codex CLI, etc.):

```
Please install the Shopfox Shopping MCP plugin for me:

1. Go to https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin and read the README
   and the .mcp.json in that repository.
2. Add an MCP server entry named "shopfox" with:
   - type: http
   - url: https://shopfox.ai/mcp
   - headers: { "Authorization": "Bearer <MY_SHOPFOX_API_KEY>" }
3. If the user doesn't have a key yet, tell them to open this link (it logs them
   in and creates a key automatically — they just copy it):
   https://shopfox.ai/user-info?createApiKey=1
4. After it's installed, ask the user a shopping question and call the tools.
   IMPORTANT — how to answer: call generate_shopping_report (or research_shopping_options)
   and output the FULL returned report to the user, verbatim. The report contains 3-5
   products with real prices, purchase links, pros/cons, and best use cases — the whole
   report IS the answer. Never summarize it, never shorten it, never pick just one
   product, and never hand the user a link to open instead.
```

The agent will read this repository, configure the MCP server for you, and tell you where to get your key. No manual JSON editing needed.

**How answers are delivered:** the tool returns a complete written report (3-5 products, real prices, purchase links, pros/cons, best use cases). The agent must relay that full report to you directly — it should never summarize it into a single pick or point you to a link.

## 🔑 Getting Your API Key

Your Shopfox API key is created in the **Shopfox web app**. Click the link below to
log in and generate a key in one step (a key is created for you automatically once
you're signed in — copy it right away, it's only shown once):

👉 **https://shopfox.ai/user-info?createApiKey=1**

The key starts with `sk_shopfox_`. You can also go to **Settings → Agent API** in the
web app at any time to generate, list, and revoke keys.

## 🚀 Manual Installation

### Claude Desktop / Claude Code

1. Get your API key (see above)
2. Add to your Claude config:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "shopfox": {
      "type": "http",
      "url": "https://shopfox.ai/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY_HERE"
      }
    }
  }
}
```

3. Restart Claude Desktop
4. Try asking: **"Find me noise-cancelling headphones under $300"**

### Any MCP-compatible client

Point your client at the same endpoint:

```
URL:      https://shopfox.ai/mcp
Type:     Streamable HTTP
Auth:     Bearer token (your sk_shopfox_ key)
Tools:    generate_shopping_report, research_shopping_options
```

### skills.sh

```bash
skills install shopfox-shopping
export SHOPFOX_API_KEY="sk_shopfox_..."
```

## 🔌 Platform Availability

| Platform | Status |
|----------|--------|
| **Any local MCP agent** (Claude Code, Cursor, Windsurf, Codex, etc.) | ✅ Available now — use the prompt above or manual config |
| **skills.sh** | ✅ Available now |
| **ChatGPT / OpenAI Apps** | 🚧 In development & review |
| **Claude Plugin Directory** | 🚧 In development & review |

Once the ChatGPT and Claude listings go live, you'll be able to install Shopfox Shopping directly from their app stores — no manual configuration. We'll update this README and post on [shopfox.ai](https://shopfox.ai) when they launch.

## 🛠️ Features

- **Real-time price comparison** across major US retailers (Amazon, Best Buy, Walmart, Target)
- **Detailed product research** with pros/cons analysis and expert reviews
- **Budget-aware recommendations** with filtering by price range and features
- **Cross-session memory** - remembers your brand preferences and shopping style
- **Two research modes**:
  - `generate_shopping_report` - One-shot complete report (preferred)
  - `research_shopping_options` - Interactive with clarification questions

## 📖 Example Prompts

### Basic Search
```
"Find me noise-cancelling headphones under $300"
"Compare wireless keyboards under $100"
"What's a good gaming laptop for $1500?"
```

### With Preferences
```
"Find a birthday gift for a photographer, budget $200"
"I need a TV for a bright living room, 55-inch, under $800"
"Recommend a robot vacuum, I have hardwood floors and a cat"
```

### With Memory
After your first search, the agent remembers your preferences:
```
First: "Find me an iPhone case, I prefer minimalist designs"
Later: "Now find me AirPods"
       → Automatically considers Apple ecosystem and minimalist style
```

More detailed walkthroughs: [examples/](./examples/)

## 🔒 Rate Limits

- **Daily quota**: 100 requests per API key
- **Concurrent requests**: 3 simultaneous requests per user
- Limits are per-owner (all your API keys share the same quota)

## 🎯 How It Works

1. **You ask** - Natural language shopping request
2. **Agent researches** - Searches across retailers, reads reviews, compares specs
3. **You get** - Structured report with 3-5 specific product recommendations, each with:
   - Product name and price
   - Direct purchase link
   - Key pros and cons
   - Best use cases

## 🆘 Support

- **Email**: support@shopfox.ai
- **Issues**: [GitHub Issues](https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin/issues)
- **Web App**: [shopfox.ai](https://shopfox.ai)

## 🔐 Privacy & Security

- Your API key is stored locally and never shared
- Search queries are processed server-side but not stored long-term
- Cross-session memory (preferences) is scoped to your user account
- See our [Privacy Policy](https://shopfox.ai/privacy) for details

## ⚠️ Limitations

- **US only** - Currently supports US retailers and shipping
- **No checkout** - We provide research and links; you complete purchases on retailer sites

## 🛣️ Roadmap

- [ ] ChatGPT / OpenAI Apps listing (in review)
- [ ] Claude Plugin Directory listing (in review)
- [ ] More countries (UK, Canada, EU)
- [ ] Multi-language reports
- [ ] Price tracking and alerts

## 📄 License

MIT License - see [LICENSE](LICENSE) file

---

**Made with ❤️ by the Shopfox team** | [shopfox.ai](https://shopfox.ai)
