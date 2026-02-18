# Polymarket Skill

Teaches AI agents how to interact with [Polymarket](https://polymarket.com) — a decentralized prediction market platform built on Polygon with a hybrid off-chain/on-chain Central Limit Order Book (CLOB).

## What This Skill Provides

The `SKILL.md` file gives your agent full context on:

- **Authentication** — Two-level auth: L1 (EIP-712 wallet signing) and L2 (HMAC-SHA256 API credentials), with TypeScript & Python examples
- **REST APIs** — Three distinct APIs: Gamma (market discovery), CLOB (trading & pricing), and Data (positions & history)
- **WebSocket APIs** — Real-time streaming via CLOB WebSocket (orderbook, trades), RTDS (crypto prices), and Sports WebSocket
- **SDKs** — Official TypeScript, Python, and Rust clients
- **Rate limits, fee structure, and error handling**
- **CTF operations** — Split, merge, and redeem outcome tokens
- **Builder Program** — Order attribution, gasless transactions, and tiered rewards for third-party integrators

## When to Use This Skill

Use the Polymarket skill when your agent needs to:

- Browse, search, and discover prediction markets and events
- Read orderbook prices, midpoints, spreads, and historical price data
- Place, cancel, or manage orders (limit, market, batch)
- Monitor positions, trade history, and portfolio state
- Stream real-time market updates via WebSocket
- Work with multi-outcome (negative risk) events
- Split, merge, or redeem conditional outcome tokens

## Getting Started

### 1. Get API Access

- **Read-only (no setup required):** All market data, prices, and orderbook endpoints are fully public — no authentication needed.
- **Trading:** You need an Ethereum private key and a funded wallet on Polygon (Chain ID 137) with USDCe.

### 2. Derive API Credentials

Polymarket uses a two-step authentication flow. Your private key (L1) is used once to derive API credentials (L2), which then authenticate all subsequent trading requests. The skill includes full examples for both TypeScript and Python. Your agent will reference these directly from `SKILL.md`.

### 3. Environment Variables

Never hardcode private keys. Store credentials as environment variables or in a `.env` file:

```
PRIVATE_KEY=0xyour-private-key
API_KEY=your-api-key
SECRET=your-api-secret
PASSPHRASE=your-api-passphrase
FUNDER_ADDRESS=0xyour-funder-address
```

## Key Concepts

| Concept | Description |
|---|---|
| **Event** | A collection of related markets grouped under a topic (e.g., "2024 US Presidential Election") |
| **Market** | A single tradeable binary outcome within an event, identified by a condition ID |
| **Token** | Represents a position in a specific outcome (Yes or No). Prices are 0.00–1.00. Winners redeem for $1 USDCe. |
| **CLOB** | Central Limit Order Book — off-chain matching with on-chain settlement |
| **Negative Risk** | Multi-outcome events where only one outcome can resolve Yes, enabling capital-efficient conversions |
| **CTF** | Conditional Token Framework — on-chain smart contracts managing outcome tokens (split, merge, redeem) |
| **Signature Type** | Wallet type identifier: `0` = EOA, `1` = Magic Link proxy, `2` = Gnosis Safe proxy |

## Three APIs at a Glance

| API | Base URL | Purpose |
|---|---|---|
| **Gamma** | `https://gamma-api.polymarket.com` | Market discovery, metadata, events, tags, search |
| **CLOB** | `https://clob.polymarket.com` | Prices, orderbooks, order placement, trading |
| **Data** | `https://data-api.polymarket.com` | User positions, trade history, activity, leaderboards |

## Tips

- **Public endpoints need no auth** — start fetching market data immediately with zero setup
- **Prices are probabilities** (0.00–1.00) — a Yes price of 0.65 means the market implies 65% probability
- **Token IDs come from Gamma** — fetch events/markets from the Gamma API, then use `clobTokenIds` for CLOB queries
- **Set `negRisk` correctly** — multi-outcome events require `negRisk: true` when creating orders
- **Use the SDK** — L1/L2 signing is complex; the TypeScript and Python clients handle it for you
- **Combine REST + WebSocket** — REST for initial state, WebSocket for real-time deltas
- **Respect rate limits** — implement backoff when throttled; use batch endpoints (`/books`, `/prices`) to reduce request count
- **Funder ≠ Signer** for proxy wallets — the funder is the proxy wallet address, not the private key's EOA
- **Check `llms.txt`** at [docs.polymarket.com/llms.txt](https://docs.polymarket.com/llms.txt) for the latest endpoint reference

## Resources

| Resource | URL |
|---|---|
| Polymarket Documentation | [docs.polymarket.com](https://docs.polymarket.com) |
| LLM Reference | [docs.polymarket.com/llms.txt](https://docs.polymarket.com/llms.txt) |
| Developer Quickstart | [docs.polymarket.com/quickstart/overview](https://docs.polymarket.com/quickstart/overview.md) |
| TypeScript SDK | [github.com/Polymarket/clob-client](https://github.com/Polymarket/clob-client) |
| Python SDK | [github.com/Polymarket/py-clob-client](https://github.com/Polymarket/py-clob-client) |
| Exchange Contract Source | [github.com/Polymarket/ctf-exchange](https://github.com/Polymarket/ctf-exchange/tree/main/src) |
| Builder Program | [docs.polymarket.com/developers/builders/builder-intro](https://docs.polymarket.com/developers/builders/builder-intro.md) |
| Polymarket Platform | [polymarket.com](https://polymarket.com) |