# attena-search

Search 60,000+ prediction markets from [Kalshi](https://kalshi.com) and [Polymarket](https://polymarket.com) — sports, crypto, politics, weather, geopolitics.

## Install as Claude Code Skill

```bash
npx skills add https://github.com/helloiamvu/attena-search
```

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
- 📖 [API docs](https://attena-api.fly.dev/docs) — OpenAPI/Swagger
- 📊 [Status](https://attena.xyz/status)

## Rate Limits

- Anonymous: 30 req/min
- API key: 300 req/min
