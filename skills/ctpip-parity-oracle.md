---
name: ctpip-parity-oracle
description: "FLUX Parity enforcement. 1 FLUX = 0.021 BTC. Pyth price feeds, OpenBook v2 market making, oracle bot architecture."
version: 1.0.0
author: Design Ledger PTY LTD
tags: [ctpip, flux, parity, oracle, solana, openbook, pyth]
---

# CTP/IP Parity Oracle

Source: R11 Book V, The Symbolic Economy

## The Parity Law

```
1 FLUX = 0.021 BTC
```

Immutable. Not market-driven. Not algorithmic. Structural.
0.021 = 2,100,000 satoshis. The geometry of 21 million.

## FLUX Token

- SPL token on Solana mainnet
- Mint: Dun6pP3Xsx9CWetKj3zd8iqHz8EYC1amYSeJKG8JzQ9n
- Decimals: 9
- Mint Authority: EVA Oracle PDA (program-controlled)
- Emitted only on validated transformation (VALID seal with Coherence Index >= 0.70)

## Price Computation

```
ask_price_SOL = 0.021 × (BTC_USD / SOL_USD)
```

Price feeds from Pyth Network (Hermes API):
- BTC/USD: e62df6c8b4a85fe1a67db44dc12de5db330f7ac66b72dc658afedf0f4a415b43
- SOL/USD: ef0d8b6fda2ceba41da15d4095d1da392a0d2f8ed0c6c7bc0f4cfac8c280b56d

Endpoint: https://hermes.pyth.network/v2/updates/price/latest

## Oracle Bot Architecture

```
Pyth Hermes API (free, REST)
    ↓ BTC/USD + SOL/USD every 60s
Oracle Bot (Vercel Cron or standalone)
    ↓ compute ask = 0.021 × BTC/SOL
    ↓ deviation check: skip if < 0.5% change
OpenBook v2 (on-chain orderbook)
    ↓ single-sided ask at Parity
Jupiter (DEX aggregator)
    ↓ discovers market, routes swaps
Any Solana Wallet
    ↓ user swaps SOL → FLUX at Parity
```

## Deviation Threshold

The bot only reprices when BTC/SOL moves more than 0.5%.
Read current ask price from OpenOrders.lockedPrice.
Compare to new computed priceLots.
Skip if within threshold. Saves SOL.

## Lot Size Math

Both FLUX and SOL are 9 decimals:
```
priceLots = price_in_SOL × baseLotSize / quoteLotSize
maxBaseLots = quantity_FLUX × 10^9 / baseLotSize
```

## OpenBook v2

- Program: opnb2LAfJYbRMAHHvqjCwQxanZn7ReEHp1k81EohpZb
- SDK: @openbook-dex/openbook-v2
- Non-custodial, permissionless
- Cancel + settle + re-place in separate transactions for reliability

## Economics

- Taker fee: 2% (goes to collectFeeAdmin)
- CVF rate: 9.5% on seal transactions
- Creator royalty: 5% on secondary sales
- Heritage max: 100 FLUX (one-time conversion of prior work)

## Implementation

Production: `sealed.energy/api/oracle-reprice` (Vercel Cron)
SDK: `@ctpip/oracle-bot`
Local: `oracle-bot.js` (Node.js standalone)
