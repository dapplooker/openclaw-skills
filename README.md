# OpenClaw Skills for DappLooker

Install these skills in [OpenClaw](https://openclaw.ai) to give your AI agent access to DappLooker's production-grade DeFi intelligence via x402 micropayments.

## What is DappLooker?

DappLooker is a DeFi intelligence suite providing real-time token metrics, perpetual market data across 10+ DEXs, multi-interval technical analysis, and AI-powered market research. These skills let any OpenClaw-compatible agent access DappLooker's capabilities through pay-per-use x402 payments.

**Live at:** https://docs.dapplooker.com

## Available Skills

| Skill | Description | Endpoints | Price |
|-------|-------------|-----------|-------|
| [dapplooker-ai](dapplooker-intel/SKILL.md) | DeFi market intelligence & research | 8 functions | $0.005 - $0.10 |

## Installation

### Option 1: Install via OpenClaw UI

1. Go to your OpenClaw dashboard
2. Navigate to Skills > Add Skill
3. Paste the raw GitHub URL of the skill file:
   ```
   https://raw.githubusercontent.com/hitesh23k/openclaw-skills/main/dapplooker-intel/SKILL.md
   ```

### Option 2: Manual Installation

Copy the contents of the `dapplooker-intel/SKILL.md` file to your OpenClaw skills directory:
```bash
cp dapplooker-intel/SKILL.md ~/.openclaw/skills/dapplooker-ai/SKILL.md
```

## Payment Setup

All DappLooker AI services require x402 payment in USDC. Supported networks:

| Network | Chain ID | Currency |
|---------|----------|----------|
| **Polygon** | 137 | USDC |
| **Base** | 8453 | USDC |

Your agent needs a funded wallet on either network to make API calls.

## How x402 Works

1. Your agent calls a DappLooker endpoint
2. The server returns a `402 Payment Required` response with payment details
3. Your agent sends USDC payment to the specified address
4. The server returns the response

OpenClaw handles steps 2-3 automatically if your agent has a funded wallet configured via `X402_PRIVATE_KEY`.

## Support

- **Documentation:** https://docs.dapplooker.com/products/api-endpoints
- **Twitter:** [@dapplooker](https://twitter.com/dapplooker)

## License

MIT License - see [LICENSE](LICENSE)
