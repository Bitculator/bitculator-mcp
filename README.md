# Bitculator MCP Server

[bitculator.com](https://bitculator.com/en/crypto-mcp) · [MCP documentation](https://bitculator.com/en/documentation/mcp/v1) · [Get an MCP key](https://bitculator.com/user/developer/mcp)
[Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers/com.bitculator%2Fmarket-data/versions/latest) · [Smithery](https://smithery.ai/servers/bitculator/market-data) · [Glama](https://glama.ai/mcp/connectors/com.bitculator/market-data)

Live and historical crypto market data for AI agents: prices, OHLCV history, sentiment indices,
technical indicators, exchanges, liquidations and conversions, as **19 read-only tools**.

It is a hosted, remote MCP server: there is nothing to install or run. Point any MCP client that
supports remote servers over Streamable HTTP at the endpoint below and add your key.

| | |
|---|---|
| Endpoint | `https://bitculator.com/mcp` |
| Transport | Streamable HTTP |
| Authentication | MCP key, as `Authorization: Bearer <key>` or `X-API-Key: <key>` |
| Registry name | `com.bitculator/market-data` |

## Get a key

Create a key in the [MCP console](https://bitculator.com/user/developer/mcp). The free MCP plan works,
no card required. MCP keys are their own product: Data API keys and widget keys are rejected with
`401 unauthenticated`.

Keep the key out of shared files you commit: put it in your client's settings or an environment variable.

## Connect

### Claude Code

```bash
claude mcp add --transport http bitculator \
    https://bitculator.com/mcp \
    --header "Authorization: Bearer YOUR_API_KEY"
```

### Cursor (`~/.cursor/mcp.json`)

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

### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "bitculator": {
      "type": "http",
      "url": "https://bitculator.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

### Claude API (MCP connector)

```bash
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: mcp-client-2025-11-20" \
  -d '{
    "model": "claude-opus-4-8",
    "max_tokens": 1024,
    "mcp_servers": [{
      "type": "url",
      "url": "https://bitculator.com/mcp",
      "name": "bitculator",
      "authorization_token": "YOUR_API_KEY"
    }],
    "tools": [{ "type": "mcp_toolset", "mcp_server_name": "bitculator" }],
    "messages": [{ "role": "user", "content": "How is the crypto market today?" }]
  }'
```

### Smithery and other gateways

On [Smithery](https://smithery.ai/servers/bitculator/market-data), enter your MCP key in the
connection form. Gateways that reserve the `Authorization` header for their own sign-in can send the
same key as an `X-API-Key` header instead.

## Tools

Every tool is read-only and publishes an `outputSchema`; results come back as text and as
`structuredContent`.

| Tool | What it does |
|---|---|
| `search_coins` | Resolve a coin by name or symbol to its slug, with a ranked market summary per match |
| `get_coin` | Full profile for one coin: supplies, rank, all-time high/low, valuation, links, contracts |
| `get_prices` | Current prices for up to 100 coins in one call, optionally converted to a fiat currency |
| `get_price_history` | OHLCV candles for a coin over a date range |
| `get_historical_price` | The price of one coin on a specific past date |
| `get_top_movers` | The biggest gainers or losers over the last 24 hours or 7 days |
| `get_trending_coins` | The coins currently drawing the most attention on Bitculator |
| `get_global_market` | Whole-market snapshot: market cap, volume, dominance, counts, Fear & Greed |
| `get_global_history` | Time series of the total crypto market cap or total volume |
| `get_fear_greed` | The Fear & Greed index for the whole market or for one coin |
| `get_altseason_index` | Whether altcoins are outperforming Bitcoin right now |
| `convert_currency` | Convert an amount between crypto and fiat currencies at live rates |
| `list_exchanges` | Ranked exchanges with 24h volume, volume dominance and pair/asset counts |
| `get_exchange` | Full profile for one exchange: rank, volume, dominance, pairs, assets, links |
| `get_exchange_trust_score` | An exchange's trust score (0-10) with its 13-factor breakdown |
| `get_coin_markets` | Where a coin trades: its tickers across exchanges, sorted by volume |
| `get_coin_technicals` | Technical snapshot for a coin: RSI, MACD, SMA, ADX, MFI, CCI, OBV, VWAP and more |
| `get_liquidations_summary` | Today's futures liquidation totals with the long/short split |
| `calculate_dca` | Backtest a dollar-cost-averaging plan against real price history |

Prices, rates and supplies are returned as decimal strings, so no precision is lost. Some tools need
a paid MCP plan; the [MCP page](https://bitculator.com/en/crypto-mcp) lists which.

## More

- [MCP documentation](https://bitculator.com/en/documentation/mcp/v1)
- [Service status](https://bitculator.com/en/status) and [API changelog](https://bitculator.com/en/documentation/api/changelog)
- Machine-readable: [`/.well-known/mcp.json`](https://bitculator.com/.well-known/mcp.json) and [`/.well-known/mcp/server-card.json`](https://bitculator.com/.well-known/mcp/server-card.json)
- Prefer plain HTTP? The same data is available through the [Data API](https://bitculator.com/en/documentation/api/v1) and its [official SDKs](https://github.com/Bitculator).

## License

The contents of this repository (documentation and configuration) are MIT licensed. The Bitculator
service itself is subject to the [Bitculator terms](https://bitculator.com/en/terms-of-service).
