# Shopfox Shopping Agent

> US shopping research and offer comparison for Claude Code, ChatGPT, Cursor, and other AI assistants — one report per request, with real prices and purchase links.

## ⚡ Easiest Way: Let Your Agent Install It

Pick your client and paste the matching prompt. The plugin is an HTTP MCP server at
`https://shopfox.ai/mcp` — the agent only needs to register it (or, for Codex, install it
from the marketplace) and set your key.

**Codex (OpenAI Codex CLI / app):**

```
Install the Shopfox Shopping plugin for me from the plugin marketplace:
1. Run: codex plugin marketplace add https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin.git
2. Run: codex plugin add shopfox-shopping@shopfox-shopping
3. I'll provide my Shopfox API key — you'll read it from the SHOPFOX_API_KEY env var.
   If I don't have a key yet, tell me to open https://shopfox.ai/user-info?createApiKey=1
   (it creates one automatically, shown once — copy it).
4. Then ask me my shopping question and call the tools.
   IMPORTANT — how to answer: call generate_shopping_report (or research_shopping_options)
   and output the FULL returned report to the user, verbatim. The report contains 3-5
   products with real prices, purchase links, pros/cons, and best use cases — the whole
   report IS the answer. Never summarize it, never shorten it, never pick just one
   product, and never hand the user a link to open instead.
```

**Claude Code / Cursor / Windsurf / other MCP agents:**

```
Install the Shopfox Shopping MCP plugin for me:
1. Add this MCP server to my local config (project .mcp.json for Claude Code, or your
   app's MCP settings) — do NOT fetch it from the internet, use exactly this:
   {
     "mcpServers": {
       "shopfox": {
         "type": "http",
         "url": "https://shopfox.ai/mcp",
         "headers": { "Authorization": "Bearer <MY_SHOPFOX_API_KEY>" }
       }
     }
   }
2. This is the official Shopfox endpoint (https://shopfox.ai/mcp); the key is only
   stored locally in my config and sent to this URL. It is safe to configure.
3. If I don't have a key yet, tell me to open https://shopfox.ai/user-info?createApiKey=1
   (it creates one automatically, shown once — copy it).
4. Then ask me my shopping question and call the tools.
   IMPORTANT — how to answer: call generate_shopping_report (or research_shopping_options)
   and output the FULL returned report to the user, verbatim. The report contains 3-5
   products with real prices, purchase links, pros/cons, and best use cases — the whole
   report IS the answer. Never summarize it, never shorten it, never pick just one
   product, and never hand the user a link to open instead.
```

No manual JSON editing needed — the agent registers the server or marketplace for you.

**How answers are delivered:** the tool returns a complete written report (3-5 products, real prices, purchase links, pros/cons, best use cases). The agent must relay that full report to you directly — it should never summarize it into a single pick or point you to a link.

## 🔑 Getting Your API Key

Your Shopfox API key is created in the **Shopfox web app**. Click the link below to
log in and generate a key in one step (a key is created for you automatically once
you're signed in — copy it right away, it's only shown once):

👉 **https://shopfox.ai/user-info?createApiKey=1**

The key starts with `sk_shopfox_`. You can also go to **Settings → Agent API** in the
web app at any time to generate, list, and revoke keys.

## 🚀 Manual Installation

### Codex (OpenAI Codex CLI / app)

Install from the plugin marketplace (one-time setup):

```bash
# Add the marketplace (GitHub source)
codex plugin marketplace add https://github.com/shopfox-ai/shopfox-shopping-mcp-plugin.git

# Install the plugin
codex plugin add shopfox-shopping@shopfox-shopping

# Set your API key (the plugin reads it from this env var)
export SHOPFOX_API_KEY="sk_shopfox_..."
```

Or add it from the Codex UI: **Codex → Plugins → Add marketplace →** paste the GitHub URL above (sparse path: `plugins/codex`). Then ask: **"Find me noise-cancelling headphones under $300"**.

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
| **Codex plugin marketplace** | ✅ Available now — `codex plugin marketplace add` (see Manual Installation) |
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
