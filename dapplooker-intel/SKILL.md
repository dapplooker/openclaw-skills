---
name: dapplooker-ai
description: DeFi intelligence suite — token metrics, perp trading data, technical analysis, smart money flows, staking yields, and AI-powered market research across Base, Solana, and 10+ perp DEXs
emoji: 🔮
homepage: https://docs.dapplooker.com/products/api-endpoints
---

# DappLooker DeFi Intelligence

Access production-grade DeFi market intelligence via DappLooker's x402 API. Get real-time token metrics with holder forensics, perpetual market data across 10+ DEXs, multi-interval technical analysis, smart money wallet tracking, staking yield intelligence, and AI-synthesized market research — all through a single unified skill.

## Payment

All endpoints require x402 payment in **USDC** on:
- **Polygon** (default settlement chain)
- **Base** (chain ID: 8453)

Payment is handled automatically via the x402 protocol headers. If not using x402, pass `api_key` as a query parameter.
<!-- Testing API Key: XXXXXXXXXXXXXXXXXXXXXX -->

---

## Functions

### token_intelligence

**URL:** `GET https://api.dapplooker.com/v1/crypto-market/`

**Important:** Trailing slash is required. Without it, the API returns a 301 redirect.

**Use when:** User wants current price, market cap, volume, holder concentration, social mindshare, dev wallet safety, or technical levels for one or more tokens.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chain` | string | Yes | Blockchain network: `base`, `solana` |
| `token_tickers` | string | No | Comma-separated tickers (e.g., `AIXBT,VIRTUAL`) |
| `token_addresses` | string | No | Comma-separated contract addresses |
| `token_ids` | string | No | Comma-separated internal IDs (e.g., `aixbt,toshi`) |
| `ecosystem` | string | No | Filter by ecosystem (e.g., `virtuals`) |
| `page` | number | No | Pagination page number |
| `api_key` | string | No | API key if not using x402 |

**Note:** Use at least one token filter (`token_tickers`, `token_addresses`, `token_ids`) or an `ecosystem` filter.

**Returns:** Array of token objects, each containing:
- `token_info` — symbol, name, contract address, chain, ecosystem
- `token_metrics` — usd_price, mcap, fdv, volume_24h, total_liquidity, circulating_supply, total_supply, price_high_24h, price_ath, price change % (1h, 24h, 7d, 30d), volume change % (7d, 30d), mcap change % (7d, 30d)
- `technical_indicators` — support, resistance, rsi, sma
- `x_social_metrics` — mindshare (3d, 7d), impression_count, engagement_count, follower_count, smart_follower_count (all with change percentages)
- `last_updated_at` — ISO timestamp

**Example prompt:** "What's the price of AIXBT?" or "Show me VIRTUAL token metrics on Base"

---

### perp_intelligence

**URL:** `GET https://api.dapplooker.com/v1/hl-perp-trade`

**Use when:** User wants real-time perpetual market data — mark price, funding rates, open interest, orderbook liquidity, or technical indicators for a specific token on a specific DEX.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `dex` | string | Yes | Venue to query. Supported: `hyperliquid`, `nado`, `paradex`, `pacifica`, `avantis`, `lighter`, `aster`, `grvt`, `variational`, `extended` |
| `token_ticker` | string | Yes | Token symbol (e.g., `BTC`, `ETH`, `SOL`, `HYPE`) |
| `api_key` | string | No | API key if not using x402 |

**Returns:** Single object containing:
- **Core:** symbol, base, quote, mark_price, mid_price, price_change_24h, price_change_percent_24h
- **Liquidity:** bid_ask_spread, bid_ask_spread_percentage_mark, bid/ask/total_liquidity_0_25_percentage, order_book_imbalance_percentage
- **Open Interest:** usd_value, change_percentage_1h/6h/24h
- **Funding:** current_value, next, trajectory (array of upcoming rates)
- **Technicals:** rsi (15m, 1h, 4h), atr (15m, 1h, 4h + percentage variants), adx (1h, 4h), obv (1h, 4h, 1d), macd_line_slope (15m, 1h, 4h — each with macd_line, signal_line, histogram)
- **Structure:** swing high/low (4h), vwap (1m, 5m, 15m, 1h, 4h, 1d), ema (20/200 distance %), bollinger (band_width, upper/lower/middle band)

**Note:** Schema is standardized across all supported DEXs. Integrate once, switch venues freely.

**Example prompt:** "What's the BTC funding rate on Hyperliquid?" or "Show me ETH perp data on Paradex"

---

### historical_market_data

**URL:** `GET https://api.dapplooker.com/v1/crypto-market-historical`

**Use when:** User wants time-series pricing, volume, and market cap data for backtesting, charting, or trend analysis.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chain` | string | Yes | Blockchain network: `base`, `solana` |
| `token_ticker` | string | No | Token ticker (use one of ticker, address, or id) |
| `token_address` | string | No | Token contract address |
| `token_id` | string | No | Internal token identifier |
| `start_date` | string | No | Start date in `YYYY-MM-DD` format |
| `end_date` | string | No | End date in `YYYY-MM-DD` format |
| `page` | number | No | Pagination page number |
| `api_key` | string | No | API key if not using x402 |

**Note:** Use exactly one token identifier: `token_ticker`, `token_address`, or `token_id`.

**Returns:** Object containing:
- `token_id`, `token_symbol`, `token_address` — token identity
- `token_day_metrics` — array of daily records, each with: date_time (YYYY-MM-DD), usd_price, total_volume, market_cap
- `meta.pagination` — page, pageSize, totalPages, totalRecords

**Example prompt:** "Show me VIRTUAL price history on Base" or "Get TOSHI historical data from January to March"

---

### token_ta

**URL:** `GET https://api.dapplooker.com/v1/token-ta`

**Use when:** User wants RSI, SMA, support/resistance levels across multiple timeframes for strategy evaluation or entry/exit zone identification.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chain` | string | Yes | Blockchain network: `base`, `solana` |
| `token_tickers` | string | No | Comma-separated tickers (e.g., `AIXBT,VIRTUAL`) |
| `token_addresses` | string | No | Comma-separated contract addresses |
| `token_ids` | string | No | Comma-separated internal IDs |
| `ecosystem` | string | No | Filter by ecosystem (e.g., `virtuals`) |
| `page` | number | No | Pagination page number |
| `api_key` | string | No | API key if not using x402 |

**Note:** Use token filters or ecosystem filter. For ecosystem-wide scans, omit token filters and set `ecosystem`.

**Returns:** Array of token objects, each containing:
- `token_id` — internal identifier
- `technical_indicators` — support and resistance at 1m, 5m, 15m, 1h, 4h, 1d, 1w intervals; rsi at same intervals; sma at same intervals
- `token_metrics` — price_change_percentage at 1m, 5m, 15m, 1h, 4h, 1d, 1w, 30d intervals
- `last_updated_at` — ISO timestamp

**Interpretation guide:**
- RSI > 70 = potentially overbought; RSI < 30 = potentially oversold
- Price above SMA = bullish bias; below = bearish bias
- Strongest setups align across 15m + 1h + 4h intervals

**Example prompt:** "What are the RSI levels for AIXBT?" or "Show me support/resistance for VIRTUAL on Base"

---

### trending_tokens

**URL:** `GET https://api.dapplooker.com/v1/trending`

**Use when:** User wants to discover hot tokens ranked by volume, market cap, smart money activity, or staking conviction.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chain` | string | Yes | Blockchain network: `base`, `solana` |
| `strategy` | string | No | Ranking model: `mcap` (default), `volume_and_staking`, `volume_and_smartmoney` |
| `api_key` | string | No | API key if not using x402 |

**Strategy descriptions:**
- `mcap` — Popular tokens with active market participation
- `volume_and_staking` — Volume + staking conviction signals
- `volume_and_smartmoney` — Volume + wallet flow signals

**Returns:** Array of ranked token objects, each containing:
- `token_id`, `token_name`, `token_symbol`, `token_address`, `network`
- `volume` — 24h trading volume in USD
- `liquidity` — current available liquidity (may be null)
- `mcap` — market capitalization in USD
- `price_change_percentage_1h`, `price_change_percentage_24h`

**Example prompt:** "What tokens are trending on Base?" or "Show me trending tokens by smart money activity"

---

### smart_money_trends

**URL:** `GET https://api.dapplooker.com/v1/smart-money-trends`

**Use when:** User wants to track net capital flows from top-performing "smart money" wallets — whale accumulation, distribution patterns, or institutional positioning.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token_tickers` | string | No | Comma-separated tickers (e.g., `VIRTUAL,AIXBT`) |
| `token_addresses` | string | No | Comma-separated contract addresses |
| `api_key` | string | No | API key if not using x402 |

**Note:** Use at least one token filter. No `chain` parameter required.

**Returns:** Array of token objects, each containing:
- `symbol`, `token_address` — token identity
- `net_inflow` — capital accumulated by smart wallets: usd_value_24h, token_24h, usd_value_7d, token_7d, usd_value_30d, token_30d
- `net_outflow` — capital sold by smart wallets: usd_value_24h, token_24h, usd_value_7d, token_7d, usd_value_30d, token_30d

**Interpretation:** Net inflow > outflow = smart money accumulating. Net outflow > inflow = smart money distributing.

**Example prompt:** "Are whales buying or selling VIRTUAL?" or "Show me smart money flows for AIXBT"

---

### staking_intelligence

**URL:** `GET https://api.dapplooker.com/v1/stake-trends`

**Use when:** User wants staking yield data, TVL, retention rates, or recent stake/unstake activity for a token.

**Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token_tickers` | string | No | Comma-separated tickers (e.g., `1000X,VIRTUAL`) |
| `token_addresses` | string | No | Comma-separated contract addresses |
| `api_key` | string | No | API key if not using x402 |

**Note:** Use at least one token filter. No `chain` parameter required.

**Returns:** Array of objects, each containing:
- `staking_details` — name, ticker, contract_address, latest_price, volume_usd_24h, price_change_percentage_24h, stake_retention_percentage, tvl (USD), net_staked (formatted string e.g., "3.4M"), net_staked_usd (formatted string e.g., "29.4K"), net_staked_percentage, total_stakers, total_withdrawn, total_withdrawn_percentage, token_holders
- `trends` — stake_count_24h, stake_amount_24h, stake_amount_usd_24h, unstake_count_24h, unstake_amount_24h, unstake_amount_usd_24h
- `staking_activities` — array of recent tx: tx_hash, wallet_address, action ("Stake"/"Unstake"), amount, timestamp, token_address, user_type ("regular", "smart_money_user", "dev")

**Example prompt:** "How much is staked in 1000X?" or "Show me staking trends for VIRTUAL"

---

### market_mcp

**URL:** `POST https://api.dapplooker.com/v1/mcp`

**Use when:** User has a complex, multi-dimensional market question that requires AI-synthesized analysis drawing from multiple data sources (token metrics, social data, staking, smart money, etc.).

**Request body (JSON):**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `question` | string | Yes | Natural language market question |
| `api_key` | string | No | API key if not using x402 |

**Example request body:**
```json
{
  "question": "What is the mindshare of AIXBT compared to VIRTUAL?",
  "api_key": "YOUR_KEY"
}
```

**Returns:** Object containing:
- `questionId` — unique question identifier
- `answerId` — unique answer identifier
- `nlpResponse` — AI-generated markdown analysis with data-backed insights, metrics, and interpretation
- `tokenDetails` — structured token data referenced in the analysis (may be null if not token-specific)

**Note:** This is a POST endpoint. Response time may be 5–15 seconds due to AI inference. The `nlpResponse` field contains a rich markdown string — present it directly to the user.

**Example prompt:** "Compare mindshare of AIXBT vs VIRTUAL" or "What's the smart money sentiment on Base ecosystem tokens?"

---

## Quick Reference

| Endpoint | Price | Method | Use Case |
|----------|-------|--------|----------|
| `/v1/crypto-market/` | $0.005 | GET | Token price, metrics, holders, social data |
| `/v1/hl-perp-trade` | $0.005 | GET | Perp funding, OI, technicals across DEXs |
| `/v1/crypto-market-historical` | $0.005 | GET | Time-series price and volume data |
| `/v1/token-ta` | $0.005 | GET | Multi-interval RSI, SMA, support/resistance |
| `/v1/trending` | $0.005 | GET | Discover hot tokens by strategy |
| `/v1/smart-money-trends` | $0.005 | GET | Whale accumulation/distribution |
| `/v1/stake-trends` | $0.005 | GET | Staking TVL, yields, retention |
| `/v1/mcp` | $0.10 | POST | AI-powered market research |

---

## Supported Chains

| Chain | Token Intel | TA | Historical | Trending | Smart Money | Staking |
|-------|-----------|-----|------------|----------|-------------|---------|
| Base | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Solana | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

## Supported Perp DEXs

Hyperliquid, Nado, Paradex, Pacifica, Avantis, Lighter, Aster, GRVT, Variational, Extended

---

## Documentation

Full API documentation: https://docs.dapplooker.com/products/api-endpoints
