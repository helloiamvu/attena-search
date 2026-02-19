# attena-search

Search 60,000+ prediction markets from [Kalshi](https://kalshi.com) and [Polymarket](https://polymarket.com) — sports, crypto, politics, weather, geopolitics.

## Use with AI Agents

### Claude Code
```bash
npx skills add https://github.com/helloiamvu/attena-search
```

### ChatGPT / GPT-4
Paste this into your system prompt or custom instructions:
> You have access to Attena Search API for prediction market data. Endpoint: `GET https://attena-api.fly.dev/api/search/?q={query}`. Returns JSON with market odds (yes_price/no_price as 0-1 probability), volume, and source URLs. Params: q (query), limit (1-200), category (sports/crypto/weather/politics/geopolitics), source (kalshi/polymarket), sort (relevance/volume/trending/closing_soon/newest), mode=lite (fast, no LLM). Full docs: https://attena.xyz/docs/search

### Gemini / Other LLMs
Paste the same instructions above, or point the agent to:
```
https://attena.xyz/llms-full.txt
```

### Cursor / Windsurf / Cline
Add to your project's `.cursorrules` or agent config:
```
For prediction market data, use the Attena Search API:
GET https://attena-api.fly.dev/api/search/?q={query}&limit=10
Docs: https://attena.xyz/docs/search
```

### Any agent with web access
Just tell it: "Use https://attena-api.fly.dev/api/search/?q= to search prediction markets. Docs at https://attena.xyz/docs/search"

## Quick Start

```bash
# Natural language search
curl "https://attena-api.fly.dev/api/search/?q=nba+tonight"

# All markets sorted by volume
curl "https://attena-api.fly.dev/api/search/?sort=volume&limit=20"

# Fast mode (no LLM, <500ms)
curl "https://attena-api.fly.dev/api/search/?q=bitcoin&mode=lite"

# Filter by source
curl "https://attena-api.fly.dev/api/search/?q=trump&source=polymarket"
```

## Parameters

| Param | Default | Description |
|-------|---------|-------------|
| `q` | `""` | Search query (natural language) |
| `limit` | `50` | Results (1–200) |
| `offset` | `0` | Pagination |
| `category` | — | `sports`, `crypto`, `weather`, `politics`, `geopolitics` |
| `source` | — | `kalshi` or `polymarket` |
| `sort` | `relevance` | `relevance`, `volume`, `trending`, `closing_soon`, `newest` |
| `mode` | — | `lite` for fast tsvector-only search |

## Response

Each result includes:
- **`yes_price`** / **`no_price`** — implied probability (0–1)
- **`volume`** — total trading volume (USD)
- **`source_url`** — direct link to the market
- **`close_time`** — when the market resolves

Full API reference: [SKILL.md](./SKILL.md)

## Links

- 🔍 [attena.xyz](https://attena.xyz) — Search UI
- 📖 [Docs](https://attena.xyz/docs/search) — API documentation
- 📋 [Swagger](https://attena-api.fly.dev/docs) — OpenAPI/Swagger
- 📊 [Status](https://attena.xyz/status)
- 🔒 [Privacy](https://attena.xyz/privacy)
- 🤖 [llms-full.txt](https://attena.xyz/llms-full.txt) — Machine-readable docs

## Rate Limits

- Anonymous: 30 req/min
- API key: 300 req/min
