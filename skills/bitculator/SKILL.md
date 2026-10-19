---
name: bitculator
description: Answer crypto market questions with live data from the Bitculator MCP server: prices, price history, Fear & Greed, technical indicators, exchange trust scores, liquidations, conversions and DCA backtests. Use when the user asks about a coin's price or trend, market sentiment, which exchanges are trustworthy, or what a recurring buy would have returned.
---

# Bitculator crypto market data

Use the Bitculator MCP server (`https://bitculator.com/mcp`) for any question that needs current or historical crypto market data. Every tool is read-only.

## Set up

1. Create a free MCP key at https://bitculator.com/user/developer/mcp (free plan: 2,500 calls a month, no card).
2. Add the server to the MCP client. It is a hosted remote server (Streamable HTTP), so nothing is installed:

```json
{
  "mcpServers": {
    "bitculator": {
      "url": "https://bitculator.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

Clients that reserve the `Authorization` header can send the same key as `X-API-Key`.

## Which tool for which question

| The user asks about | Use |
|---|---|
| A coin by name or ticker | `search_coins` first, to get the slug; then `get_coin` for the profile |
| Current price(s), optionally in a fiat currency | `get_prices` (up to 100 coins per call) |
| Price on a past date | `get_historical_price` |
| Chart data, OHLCV candles | `get_price_history` |
| Biggest gainers or losers, what is trending | `get_top_movers`, `get_trending_coins` |
| The whole market: market cap, volume, dominance | `get_global_market`, `get_global_history` |
| Sentiment | `get_fear_greed` (market or one coin), `get_altseason_index` |
| RSI, MACD, moving averages and similar | `get_coin_technicals` |
| Where a coin trades | `get_coin_markets` |
| Exchanges and how trustworthy they are | `list_exchanges`, `get_exchange`, `get_exchange_trust_score` (0-10 with a 13-factor breakdown) |
| Futures liquidations today | `get_liquidations_summary` |
| Converting an amount between crypto and fiat | `convert_currency` |
| What a recurring buy would have returned | `calculate_dca` |

## How to answer well

- Resolve the coin to its slug with `search_coins` before calling other tools; do not guess slugs.
- Prices, rates and supplies come back as decimal strings. Keep them as strings when doing arithmetic or quoting them, so no precision is lost.
- State the data source and the timestamp or date range with the answer.
- Some tools need a paid MCP plan. If a call is refused for plan reasons, say so and point the user to https://bitculator.com/en/crypto-mcp instead of retrying.
- The data is informational. Do not present an indicator or a sentiment reading as financial advice or a prediction.

## Reference

- MCP documentation: https://bitculator.com/en/documentation/mcp/v1
- Service status: https://bitculator.com/en/status
- Prefer plain HTTP? The same data is in the Data API: https://bitculator.com/en/documentation/api/v1
