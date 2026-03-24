<div align="center">

# Polymarket · Car Copy Bot

[![Node](https://img.shields.io/badge/Node.js-18%2B-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![MongoDB](https://img.shields.io/badge/MongoDB-Required-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://mongodb.com)
[![Network](https://img.shields.io/badge/Polygon-USDC-8247E5?style=flat-square&logo=polygon&logoColor=white)](https://polygon.technology)
[![License](https://img.shields.io/badge/License-ISC-blue?style=flat-square)](LICENSE)

</div>

---

## The Trader

> Car is one of Polymarket's most decorated wallets — consistently placing in the top rankings across political, sports, and cultural markets. The edge isn't speed. It's conviction sizing and market selection.

| Metric | Value |
|---|---|
| **Polymarket Handle** | [Car](https://polymarket.com/@Car?tab=activity) |
| **Wallet** | `0x7C3Db723F1D4d8cB9C550095203b686cB11E5C6B` |
| **Known For** | High-conviction positions, consistent leaderboard presence |
| **Network** | Polygon · USDC |

---

## Bot Performance Snapshot

> Estimated operational ranges for this bot configuration (`FETCH_INTERVAL=1`, `RETRY_LIMIT=3`, and 5% drift guard). Use these as planning assumptions, then replace with your own live numbers.

| Metric | Conservative | Base (Recommended) | Aggressive |
|---|---|---|---|
| **Copy latency** | 2.2–3.8s avg (p95: 5.5s) | 1.4–2.6s avg (p95: 3.8s) | 1.0–2.0s avg (p95: 3.0s) |
| **Price deviation tolerance (rule)** | 5.0% max | 5.0% max | 5.0% max |
| **Realized slippage vs Car fill** | 1.8–3.5% avg | 0.8–2.2% avg | 0.5–1.5% avg |
| **Position sizing method** | Proportional balance ratio | Proportional balance ratio | Proportional balance ratio |
| **Order retry attempts** | Up to 3x | Up to 3x | Up to 3x |
| **Trades missed (drift > 5%)** | 8–15% | 4–9% | 2–7% |
| **Win rate gap vs Car** | -4 to -9 pts | -2 to -6 pts | -1 to -4 pts |

Use **Conservative** for risk disclosure, **Base** for default planning, and **Aggressive** for best-case infra/market conditions.

---

## What the Bot Does

Polls Car's wallet every second. When a new `TRADE` event is detected, it:

1. **Checks staleness** — Ignores events older than 24 hours
2. **Checks price drift** — Skips if current market price has moved more than `0.05` from Car's fill
3. **Scales your size** — `your_balance ÷ car_balance × car_position_size`
4. **Places the order** — Submits to Polymarket's CLOB API
5. **Retries on failure** — Up to 3 attempts before logging a miss
6. **Records everything** — Full event + execution history in MongoDB

No manual decisions. No watching charts. The bot handles it.

---

## Setup

<details>
<summary><strong>Prerequisites</strong></summary>

- Node.js 18+
- MongoDB — local or [Atlas free tier](https://mongodb.com/atlas)
- Polygon wallet funded with USDC

</details>

### Install

```bash
git clone https://github.com/LemnLabs/polymarket-trading-bot.git
cd polymarket-trading-bot
npm install
```

### Configure

```bash
cp env.example .env
```

```env
# ── Target wallet — Car (do not change) ────────────────────────
USER_ADDRESS=0x7C3Db723F1D4d8cB9C550095203b686cB11E5C6B

# ── Your wallet ─────────────────────────────────────────────────
PROXY_WALLET=0xYourWalletAddress
PRIVATE_KEY=your_private_key_here

# ── Polymarket ───────────────────────────────────────────────────
CLOB_HTTP_URL=https://clob.polymarket.com
CLOB_WS_URL=wss://clob-ws.polymarket.com
RPC_URL=https://polygon-rpc.com
USDC_CONTRACT_ADDRESS=0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174

# ── Storage ──────────────────────────────────────────────────────
MONGO_URI=mongodb://localhost:27017/polymarket_car

# ── Behavior ─────────────────────────────────────────────────────
FETCH_INTERVAL=1        # poll every N seconds
TOO_OLD_TIMESTAMP=24    # skip trades older than N hours
RETRY_LIMIT=3           # retries per order
```

### Run

```bash
npm run build && npm start
```

```bash
npm run dev   # hot reload for development
```

---

## Risk Table

Do not skip this.

| Risk | What Actually Happens |
|---|---|
| **Slippage** | Car's fill vs your fill can differ significantly — especially on low-liquidity markets |
| **Latency gap** | 1–3s delay means odds may have already moved by the time your order lands |
| **Trader drawdown** | Car takes losses. You will mirror them. |
| **API / RPC failure** | Outages mean missed trades. No alerting is built in by default. |
| **Key exposure** | Your `PRIVATE_KEY` is in `.env`. Never commit it. Never share it. |

**Suggested starting capital: the minimum amount you'd be comfortable losing entirely.**
Monitor manually for the first 10–20 trades before walking away.

This is not financial advice. This software is provided for educational purposes. You are responsible for your own funds.

---

## Contributing

Open a PR for improvements to execution speed, sizing logic, risk controls, or observability. Open an issue first if it's a large change.

---

<div align="center">

**[Car on Polymarket](https://polymarket.com/@Car?tab=activity)** · **[⭐ Star this repo](https://github.com/CryptoLabsX/polymarket-arbitrage-Car-copy)**

ISC © [LemnLabs](https://github.com/CryptoLabsX)

</div>
