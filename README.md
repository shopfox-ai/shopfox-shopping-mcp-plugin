# Shopfox Shopping Agent

> US shopping research and offer comparison for Claude Code, ChatGPT, and other AI assistants.

## 🚀 Quick Start

### For Claude Code / Claude Desktop

1. Get your API key from [shopfox.ai/settings](https://shopfox.ai/settings) (Desktop app → Settings → Agent API)

2. Add to Claude Desktop config:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "shopfox": {
      "type": "http",
      "url": "https://mcp.shopfox.ai/mastra/mcp/shopfox/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY_HERE"
      }
    }
  }
}
```

3. Restart Claude Desktop

4. Try asking: **"Find me noise-cancelling headphones under $300"**

### For skills.sh

```bash
skills install shopfox-shopping
```

Then set your API key:
```bash
export SHOPFOX_API_KEY="sk_shopfox_..."
```

### For ChatGPT

Install from [ChatGPT Plugin Store](https://chatgpt.com) (coming soon)

## 🛠️ Features

- **Real-time price comparison** across major US retailers (Amazon, Best Buy, Walmart, Target)
- **Detailed product research** with pros/cons analysis and expert reviews
- **Budget-aware recommendations** with filtering by price range and features
- **Cross-session memory** - remembers your brand preferences and shopping style
- **Two research modes**:
  - `research_shopping_options` - Interactive with clarification questions
  - `generate_shopping_report` - One-shot complete report

## 📖 Example Prompts

### Basic Search
```
"Compare wireless keyboards under $100"
"Find the best noise-cancelling headphones"
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

## 🔧 Configuration

### Environment Variables

```bash
# Required: Your Shopfox API key
SHOPFOX_API_KEY="sk_shopfox_..."

# Optional: Custom MCP server URL (default: https://mcp.shopfox.ai)
SHOPFOX_MCP_URL="https://your-server.com"
```

### Rate Limit Configuration

Rate limits can be configured via environment variables on the server side:

- `MCP_DAILY_CALL_LIMIT` - Daily requests per user (default: 100)
- `MCP_CONCURRENCY_LIMIT` - Concurrent requests per user (default: 3)

## 📚 Documentation

- [Full Documentation](https://docs.shopfox.ai/mcp) (coming soon)
- [API Reference](https://docs.shopfox.ai/api) (coming soon)
- [Example Use Cases](./examples/)

## 🆘 Support

- **Email**: support@shopfox.ai
- **Issues**: [GitHub Issues](https://github.com/shopfox-ai/shopfox-mcp-plugin/issues)
- **Web App**: [shopfox.ai](https://shopfox.ai)

## 🔐 Privacy & Security

- Your API key is stored locally and never shared
- Search queries are processed server-side but not stored long-term
- Cross-session memory (preferences) is scoped to your user account
- See our [Privacy Policy](https://shopfox.ai/privacy) for details

## ⚠️ Limitations

- **US only** - Currently supports US retailers and shipping
- **English only** - Reports are generated in English
- **No checkout** - We provide research and links; you complete purchases on retailer sites

## 🛣️ Roadmap

- [ ] Support for more countries (UK, Canada, EU)
- [ ] Multi-language reports
- [ ] Price tracking and alerts
- [ ] Saved searches and watchlists
- [ ] Integration with more AI platforms

## 📄 License

MIT License - see [LICENSE](LICENSE) file

---

**Made with ❤️ by the Shopfox team** | [shopfox.ai](https://shopfox.ai)
