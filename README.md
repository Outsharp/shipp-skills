# Shipp Skills

Skills for [Shipp](https://shipp.ai) — the real-time data connector for AI builders.

Available on [ClawHub](https://clawhub.ai/kclonts/shipp)

To install, run in your terminal (Or ask Claude, Codex etc.)
```bash
npx skills add outsharp/shipp-skills
```

---

## What Are These Skills?

Shipp Skills teach AI agents (Claude Code, coding assistants, autonomous agents) how to integrate with real-time data APIs. Each skill provides context, authentication patterns, endpoint references, and usage guidance so an agent can build working integrations without guesswork.

## Available Skills

| Skill | Description |
|---|---|
| [shipp](./skills/shipp/) | Real-time data connector — fetch live event streams (sports, and more) via the Shipp API |
| [kalshi](./skills/kalshi/) | Regulated prediction market exchange — market data, trading, portfolio management, WebSocket streaming |
| [metaculus](./skills/metaculus/) | Forecasting platform — browse questions, submit forecasts, read community predictions, CDF generation, comments |
| [polymarket](./skills/polymarket/) | Decentralized prediction market on Polygon — market discovery, CLOB trading, orderbooks, WebSocket streaming, CTF operations |
| [google-news](./skills/google-news/) | Google News RSS feeds — top stories, topic feeds, keyword search, and multi-region news aggregation (no API key required) |
| [openfootball](./skills/openfootball/) | Football (soccer) match data — fixtures, results, and scores for Premier League, Bundesliga, La Liga, Serie A, Ligue 1, World Cup, and more (no API key required) |

## Quick Start

1. **Get your API keys**
   - [Shipp API Key](https://platform.shipp.ai/signup) — for real-time data
   - [Kalshi API Key](https://help.kalshi.com/account/demo-account) — for prediction market trading (demo account recommended)
   - [Metaculus API Token](https://www.metaculus.com/accounts/settings/account/#api-access) — for forecasting platform access (all endpoints require auth)
   - [Polymarket Wallet](https://docs.polymarket.com/quickstart/overview.md) — for decentralized prediction market trading (Ethereum private key + funded Polygon wallet with USDCe; read-only market data requires no auth)
   - [Google News](https://news.google.com) — no API key or account needed; all RSS feeds are fully public
   - [openfootball](https://github.com/openfootball/football.json) — no API key or account needed; all JSON data is fully public

2. **Add the skills** to your agent's context via [ClawHub](https://clawhub.ai/kclonts/shipp) or by referencing the `SKILL.md` files directly.

3. **Build something.** The skills give your agent everything it needs to call the APIs, handle auth, and process responses.

## Example Project: Alph Bot

[**Alph Bot**](https://gitlab.com/outsharp/shipp/alph-bot) is an open-source trading bot that demonstrates both skills working together in a real application. It uses Shipp for live sports data and Kalshi for prediction market trading — with Claude AI estimating probabilities in between.

### What Alph Bot Does

- Fetches real-time game events from **Shipp** (NBA, NFL, MLB, NHL, Soccer)
- Uses **Claude AI** to estimate outcome probabilities from live game state
- Identifies mispriced markets on **Kalshi** using value betting strategy
- Executes trades automatically with configurable risk management

### How It Uses the Skills

**Shipp skill in practice** — Alph Bot creates a Shipp connection for a specific game, then polls for live events using cursor-based pagination (`since_event_id`). The schema-flexible `data[]` responses are fed directly to Claude for analysis.

**Kalshi skill in practice** — Alph Bot searches Kalshi markets related to the game, reads orderbook prices, compares them against AI-estimated probabilities, and places orders when it finds sufficient edge.

### Try It

```
git clone https://gitlab.com/outsharp/shipp/alph-bot.git
cd alph-bot

cp .env.example .env
# Add your API keys to .env

yarn migrate

# List available games
./index.ts available-games --sport NBA

# Run value betting on a specific game (paper mode)
./index.ts value-bet -d --paper --game <GAME_ID>
```

> **Warning:** Alph Bot involves risking real money when not in paper/demo mode. Always test thoroughly before live trading.

## Documentation & Resources

| Resource | URL |
|---|---|
| Shipp Docs | [docs.shipp.ai](https://docs.shipp.ai) |
| Shipp How-To Guide | [docs.shipp.ai/how-to](https://docs.shipp.ai/how-to/) |
| Shipp API Reference | [docs.shipp.ai/api-reference](https://docs.shipp.ai/api-reference/) |
| Shipp Dashboard | [platform.shipp.ai](https://platform.shipp.ai) |
| Kalshi Docs | [docs.kalshi.com](https://docs.kalshi.com) |
| Kalshi LLM Docs | [docs.kalshi.com](https://docs.kalshi.com/llms.txt) |
| Metaculus API Docs | [metaculus.com/api](https://www.metaculus.com/api/) |
| Polymarket Docs | [docs.polymarket.com](https://docs.polymarket.com) |
| Polymarket LLM Docs | [docs.polymarket.com/llms.txt](https://docs.polymarket.com/llms.txt) |
| Google News RSS (US) | [news.google.com/rss?hl=en-US&gl=US&ceid=US:en](https://news.google.com/rss?hl=en-US&gl=US&ceid=US:en) |
| openfootball / football.json | [github.com/openfootball/football.json](https://github.com/openfootball/football.json) |
| openfootball GitHub Pages | [openfootball.github.io](https://openfootball.github.io) |
| Alph Bot | [gitlab.com/outsharp/shipp/alph-bot](https://gitlab.com/outsharp/shipp/alph-bot) |

## License

See [LICENSE](./LICENSE).
