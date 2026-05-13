# OpenClaw Skills for DappLooker

Install these skills in [OpenClaw](https://openclaw.ai) to give your AI agent access to DappLooker's production-grade DeFi intelligence via x402 micropayments.

## What is DappLooker?

DappLooker is a DeFi intelligence suite providing real-time token metrics, perpetual market data across 10+ DEXs, multi-interval technical analysis, and AI-powered market research. These skills let any OpenClaw-compatible agent access DappLooker's capabilities through pay-per-use x402 payments.

**Live at:** https://docs.dapplooker.com

## Available Skills

| Skill | Description | Endpoints | Price |
|-------|-------------|-----------|-------|
| [dapplooker-ai](dapplooker-intel/SKILL.md) | DeFi market intelligence & research | 8+ functions | $0.005 - $0.50 |

For a detailed breakdown of all endpoints and their specific costs, see [API_PRICING.md](API_PRICING.md).

## Installation

### Option 1: Setup via GitHub (Recommended)

You can install the DappLooker skills directly from this GitHub repository using the OpenClaw CLI or UI.

#### Via OpenClaw CLI
If you have the `openclaw` CLI installed, run:
```bash
openclaw skill add https://raw.githubusercontent.com/hitesh23k/openclaw-skills/main/dapplooker-intel/SKILL.md
```

#### Via OpenClaw UI
1. Go to your OpenClaw dashboard
2. Navigate to **Skills** > **Add Skill**
3. Paste the raw GitHub URL:
   ```
   https://raw.githubusercontent.com/hitesh23k/openclaw-skills/main/dapplooker-intel/SKILL.md
   ```

### Option 2: Manual Installation

If you prefer a manual setup, copy the skill definition to your local OpenClaw configuration:

```bash
mkdir -p ~/.openclaw/skills/dapplooker-ai
cp dapplooker-intel/SKILL.md ~/.openclaw/skills/dapplooker-ai/SKILL.md
```

## Payment Setup

All DappLooker AI services require x402 payment in USDC. 

### Supported Networks
| Network | Chain ID | Currency |
|---------|----------|----------|
| **Polygon** | 137 | USDC |
| **Base** | 8453 | USDC |

### Configuration
Your agent needs a funded wallet on either network. Configure your private key in your environment:
```bash
export X402_PRIVATE_KEY=your_private_key_here
```

## How x402 Works

1. **Request:** Your agent calls a DappLooker endpoint.
2. **Challenge:** The server returns `402 Payment Required` with a payment challenge.
3. **Payment:** OpenClaw automatically signs a USDC transaction and sends it to the settlement network.
4. **Response:** The server verifies the payment and returns the requested DeFi data.

## OpenClaw x402 Wrapper Proxy

DappLooker's API provides payment challenges within the JSON response body. However, OpenClaw's automated payer expects these challenges to be present in the HTTP headers (e.g., `X-Payment`).

## Distribution & Zero-Friction Strategy

To provide a seamless experience for OpenClaw users without requiring them to run local proxies or changing the core API backend, we recommend the following distribution model:

### 1. DappLooker Hosted Gateway (Recommended)
DappLooker hosts a public instance of the `x402-wrapper` (e.g., at `https://gateway.dapplooker.com`). This gateway serves as the "OpenClaw-compatible" entry point.

### 2. Update Skill Definitions
The `SKILL.md` distributed to users should point to this hosted gateway URL by default. 

**Example in SKILL.md:**
```markdown
**URL:** `GET https://gateway.dapplooker.com/v1/mcp/`
```

### 3. Benefit
- **End-User:** Zero friction. They install the skill via GitHub URL, and it "just works" because the hosted gateway handles the x402 header injection required by OpenClaw.
- **DappLooker:** No changes required to the production backend code. The wrapper acts as a lightweight adapter layer.

---

## Local Development (Optional)

If you are developing or testing locally, you can still use the provided wrapper:

1. Start the proxy: `./start_wrapper.sh`
2. Point your local skill to `http://localhost:3627`.

## Support

- **Documentation:** [docs.dapplooker.com](https://docs.dapplooker.com/products/api-endpoints)
- **Twitter:** [@dapplooker](https://twitter.com/dapplooker)

## License

MIT License - see [LICENSE](LICENSE)
