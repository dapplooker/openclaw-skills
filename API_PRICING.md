# DappLooker AI API Pricing & Endpoints

DappLooker AI provides a production-grade DeFi intelligence layer for autonomous agents and developers. We use the **x402 Micropayment Protocol** to enable seamless, pay-per-use access to our data using USDC.

## Pricing Model

- **Pay-per-use:** No monthly subscriptions. You only pay for the data you consume.
- **Currency:** USDC
- **Settlement Networks:**
    - **Polygon (Mainnet):** Chain ID 137 (Default)
    - **Base:** Chain ID 8453
- **Automatic Handling:** OpenClaw agents handle x402 payments automatically if a funded wallet is configured.

## API Endpoint Breakdown

| Endpoint | Method | Base Price | Description |
|----------|--------|------------|-------------|
| `/v1/mcp/` | POST | **$0.100** | AI-synthesized analysis drawing from multiple data sources. |
| `/v1/live-perp-strategy/` | GET | **$0.500** | Advanced AI-generated perp trading strategies and signals. |
| `/v1/crypto-market/` | GET | **$0.005** | Real-time token metrics, price, market cap, volume, and social mindshare. |
| `/v1/hl-perp-trade/` | GET | **$0.005** | Execution-grade perp data: funding rates, OI, and orderbook liquidity. |
| `/v1/token-ta/` | GET | **$0.005** | Multi-interval technical analysis (RSI, SMA, Support/Resistance). |
| `/v1/trending/` | GET | **$0.005** | Hot tokens ranked by volume, smart money, or staking activity. |
| `/v1/smart-money-trends/` | GET | **$0.005** | Institutional-grade capital flow tracking (Whale accumulation). |
| `/v1/stake-trends/` | GET | **$0.005** | Staking yield intelligence, TVL, and retention metrics. |
| `/v1/crypto-market-historical/` | GET | **$0.005** | Time-series historical price, volume, and mcap data. |
| `/v1/agent-metrics/` | GET | **$0.005** | Detailed metrics and holder forensics for specific agents/tokens. |
| `/v1/wallet-safety-score/` | GET | **$0.005** | Risk assessment and safety scores for smart contracts and wallets. |
| `/v1/oi-data/` | GET | **$0.005** | Aggregate Open Interest data across multiple perpetual exchanges. |
| `/v1/bubble-map/` | GET | **$0.005** | Visual representation of token holder relationships and clusters. |
| `/v1/trending-tokens/` | GET | **$0.005** | Discovery of emerging tokens based on social and on-chain signals. |
| `/v1/perp-trading-research/` | POST | **$0.005** | Deep research and quantitative analysis for perpetual markets. |
| `/v1/participant-trends/` | GET | **$0.005** | Tracking activity levels of different market participant types. |
| `/v1/monthly-revenue/` | GET | **$0.005** | Protocol and token-specific revenue generation metrics. |

## Network Support

| Feature | Base | Solana | Ethereum |
|---------|------|--------|----------|
| Token Metrics | ✅ | ✅ | ✅ |
| Technical Analysis | ✅ | ✅ | ✅ |
| Smart Money Flows | ✅ | ❌ | ❌ |
| Staking Yields | ✅ | ❌ | ❌ |
| Historical Data | ✅ | ✅ | ✅ |

## How to use x402 Payments

1. **Configure Wallet:** Set your `X402_PRIVATE_KEY` in your agent's environment.
2. **Fund Wallet:** Ensure you have USDC on Polygon or Base.
3. **Make Request:** Call any endpoint. If a `402 Payment Required` is returned, OpenClaw will:
    - Parse the payment challenge.
    - Sign and send the USDC transaction.
    - Retry the request with the payment proof.
    - Return the data seamlessly.

For more details, visit our [Developer Portal](https://docs.dapplooker.com/products/api-endpoints).
