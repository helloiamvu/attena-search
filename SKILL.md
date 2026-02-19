# Attena Search — Prediction Market Search API

Search 60,000+ prediction markets across Kalshi and Polymarket. Sports, crypto, politics, weather, geopolitics — all in one query.

## Quick Start

```bash
curl "https://attena-api.fly.dev/api/search/?q=nba+tonight"
```

## API Reference

### Endpoint

```
GET https://attena-api.fly.dev/api/search/
```

### Parameters

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| `q` | string | `""` | Search query. Natural language works: "Lakers tonight", "bitcoin above 100k", "Fed rate cut odds" |
| `limit` | int | `50` | Results to return (1–200) |
| `offset` | int | `0` | Pagination offset |
| `category` | string | — | Filter: `sports`, `crypto`, `weather`, `politics`, `geopolitics` |
| `source` | string | — | Filter: `kalshi` or `polymarket` |
| `sort` | string | `relevance` | Sort: `relevance`, `volume`, `trending`, `closing_soon`, `newest` |
| `mode` | string | — | `lite` = fast tsvector-only search (no LLM, <500ms) |

### Response

```json
{
  "query": "nba tonight",
  "results": [
    {
      "id": "abc123",
      "title": "Lakers vs Celtics: Winner",
      "category": "sports",
      "subcategory": "nba",
      "league": "nba",
      "event_date": "2026-02-19",
      "source": "polymarket",
      "market_id": "1234567",
      "yes_price": 0.65,
      "no_price": 0.35,
      "volume": 125000,
      "volume_24h": 8500,
      "rank": 0.95,
      "source_url": "https://polymarket.com/event/...",
      "close_time": "2026-02-20T04:00:00Z",
      "bracket_count": 1,
      "outcome_label": "Los Angeles Lakers",
      "ticker": "KXNBA-LAL-BOS"
    }
  ],
  "total_count": 42,
  "search_tier": "vertex",
  "timing_ms": 320
}
```

### Key Fields

- **`yes_price`** / **`no_price`**: Probability (0–1). `yes_price: 0.65` means 65% implied probability.
- **`volume`**: Total trading volume in USD.
- **`source_url`**: Direct link to the market on Kalshi or Polymarket.
- **`bracket_count`**: Number of outcome brackets (>1 for multi-outcome markets).

## Usage Patterns

### Browse all markets by volume
```bash
curl "https://attena-api.fly.dev/api/search/?sort=volume&limit=20"
```

### Filter by source
```bash
curl "https://attena-api.fly.dev/api/search/?q=bitcoin&source=kalshi"
curl "https://attena-api.fly.dev/api/search/?q=bitcoin&source=polymarket"
```

### Filter by category
```bash
curl "https://attena-api.fly.dev/api/search/?q=tonight&category=sports"
curl "https://attena-api.fly.dev/api/search/?category=crypto&sort=volume"
```

### Fast mode (skip LLM, <500ms)
```bash
curl "https://attena-api.fly.dev/api/search/?q=nba&mode=lite"
```

Use `mode=lite` when your query is already structured (exact keywords, no natural language needed). Default mode uses an LLM to understand intent — better results but ~1-3s.

### Closing soon
```bash
curl "https://attena-api.fly.dev/api/search/?sort=closing_soon&limit=10"
```

### Pagination
```bash
curl "https://attena-api.fly.dev/api/search/?q=crypto&limit=20&offset=0"   # Page 1
curl "https://attena-api.fly.dev/api/search/?q=crypto&limit=20&offset=20"  # Page 2
```

## Rate Limits

- Anonymous: 30 requests/minute
- With API key: 300 requests/minute

## Tips for AI Agents

1. **Use `mode=lite`** for structured queries to save 1-2s of latency.
2. **Default mode** is better for natural language ("who's favored in the Super Bowl?").
3. **Empty `q` + `sort=volume`** gives you the biggest markets across all categories.
4. **`yes_price`** is the market's implied probability — multiply by 100 for percentage.
5. **Check `source_url`** to link users directly to the trading platform.
6. **`total_count`** tells you if there are more results to paginate through.

## Categories

| Category | What's in it | Example queries |
|----------|-------------|-----------------|
| `sports` | NBA, NFL, NHL, F1, soccer, esports | "Lakers tonight", "Super Bowl winner" |
| `crypto` | BTC, ETH, SOL, token prices, ETF approvals | "Bitcoin above 100K", "Ethereum this week" |
| `politics` | Elections, policy, approval ratings | "Trump odds", "UK elections" |
| `weather` | Temperature, storms, climate | "NYC high temp tomorrow", "hurricane season" |
| `geopolitics` | Conflicts, treaties, international relations | "Ukraine ceasefire", "Taiwan conflict" |

## Links

- **Search UI**: [attena.xyz](https://attena.xyz)
- **API docs**: [attena-api.fly.dev/docs](https://attena-api.fly.dev/docs)
- **Status**: [attena.xyz/status](https://attena.xyz/status)
