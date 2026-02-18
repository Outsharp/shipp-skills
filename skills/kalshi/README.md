# Kalshi Skill

Teaches AI agents how to interact with the [Kalshi](https://kalshi.com) prediction market exchange — the first regulated exchange for event contracts in the US.

## What This Skill Provides

The `SKILL.md` file gives your agent full context on:

- **Authentication** — RSA-PSS request signing (Python & JavaScript examples)
- **REST API** — Markets, events, orders, portfolio, orderbook, historical data
- **WebSocket API** — Real-time streaming for orderbook updates, trades, and ticker data
- **SDKs** — Official Python and TypeScript clients
- **Rate limits, pagination, and error handling**

## When to Use This Skill

Use the Kalshi skill when your agent needs to:

- Browse and search prediction markets
- Read orderbook prices and market data
- Place, cancel, or manage orders
- Monitor positions and portfolio state
- Stream real-time market updates via WebSocket

## Getting Started

### 1. Get API Access

- **Demo account (recommended first):** [Create a demo account](https://help.kalshi.com/account/demo-account) on Kalshi to test the integration risk-free.
- **Production account:** Sign up at [kalshi.com](https://kalshi.com) when you're ready to trade with real money.

### 2. Generate API Keys

Generate an RSA key pair and register the public key with Kalshi. The skill includes signing examples for both Python and JavaScript. Your agent will reference these directly from `SKILL.md`.

### 3. Environment Variables

Never hardcode API keys. Store credentials as environment variables or in a `.env` file:

```
KALSHI_API_KEY_ID=your-key-id
KALSHI_PRIVATE_KEY_PATH=./keys/kalshi-private.pem
```

## Example Project: Alph Bot

[**Alph Bot**](https://gitlab.com/outsharp/shipp/alph-bot) is an open-source trading bot that uses the Kalshi skill alongside the Shipp skill to build an automated prediction market trader.

### How Alph Bot Uses Kalshi

1. **Market discovery** — Searches for Kalshi event contracts related to a live sports game (e.g., NBA point spreads, totals)
2. **Price reading** — Fetches orderbook data to determine current market-implied probabilities
3. **Edge detection** — Compares Kalshi market prices against AI-estimated probabilities (powered by Claude + Shipp real-time data)
4. **Order execution** — Places limit orders when sufficient edge is found, respecting position limits and risk parameters
5. **Risk management** — Enforces circuit breakers (max daily loss), position size limits, and minimum account balance thresholds

### Alph Bot's Kalshi Configuration

From its `.env.example`:

```
ALPH_BOT_KALSHI_API_KEY_ID=abc123
ALPH_BOT_KALSHI_PRIVATE_KEY_PATH=./keys/kalshi-private.pem

# Risk controls
ALPH_BOT_MAX_TOTAL_EXPOSURE_USD=10000
ALPH_BOT_MAX_POSITION_SIZE_USD=1000
ALPH_BOT_MAX_SINGLE_MARKET_PERCENT=20
ALPH_BOT_MAX_DAILY_LOSS_USD=500
ALPH_BOT_MAX_DAILY_TRADES=50
ALPH_BOT_MIN_ACCOUNT_BALANCE_USD=100
```

### Running Alph Bot with Kalshi

```
# Clone the project
git clone https://gitlab.com/outsharp/shipp/alph-bot.git
cd alph-bot

cp .env.example .env
# Fill in your Kalshi, Shipp, and Anthropic API keys

yarn migrate

# Find a game to trade on
./index.ts available-games --sport NBA

# Run value betting in demo mode
./index.ts value-bet -d --game <GAME_ID>

# Run in paper mode (no real orders)
./index.ts value-bet --paper --game <GAME_ID>
```

> **Warning:** Trading on Kalshi involves real money. Always start with a [demo account](https://help.kalshi.com/account/demo-account) or `--paper` mode.

## Key Concepts

| Concept | Description |
|---|---|
| **Event** | A real-world question (e.g., "Will the Lakers win tonight?") |
| **Market** | A specific contract under an event with yes/no outcome |
| **Price** | In cents (1–99), representing implied probability percentage |
| **Orderbook** | Bids only — derive asks via `100 - price` complement |
| **Series** | A group of related events (e.g., all NBA game markets) |

## Tips

- **Use the demo environment** for all development and testing
- **Prices are in cents** (1–99) — a yes price of 65 means the market implies 65% probability
- **Combine REST + WebSocket** — REST for initial state, WebSocket for real-time deltas
- **Handle pagination** — always check for a non-null `cursor` and loop until exhausted
- **Respect rate limits** — implement exponential backoff on 429 responses
- **Check `llms.txt`** at [docs.kalshi.com/llms.txt](https://docs.kalshi.com/llms.txt) for the latest endpoint reference

## Resources

| Resource | URL |
|---|---|
| Kalshi Documentation | [docs.kalshi.com](https://docs.kalshi.com) |
| LLM Reference | [docs.kalshi.com/llms.txt](https://docs.kalshi.com/llms.txt) |
| OpenAPI Spec | [docs.kalshi.com/openapi.yaml](https://docs.kalshi.com/openapi.yaml) |
| Demo Account Setup | [help.kalshi.com/account/demo-account](https://help.kalshi.com/account/demo-account) |
| Alph Bot (example project) | [gitlab.com/outsharp/shipp/alph-bot](https://gitlab.com/outsharp/shipp/alph-bot) |