# Trading Overview

Addax powers decentralized leveraged trading across crypto, commodities, and equities. This page explains how the trading interface works and how a trade is priced and settled.

## The trading model

Addax is a **synthetic, oracle-priced** trading venue. When you open a position you are not buying an asset or matching a counterparty on an order book, you are opening a leveraged bet against the **collateral vault** for your chosen collateral. Your PnL is paid from (or into) that vault.

This means:

- **Uniform liquidity.** Every market can be traded at size because liquidity comes from the vault, not resting orders.
- **No book slippage.** Execution price is the oracle mark price adjusted by spread and price impact, not by how thin an order book is.
- **Any priceable market.** If there's a reliable oracle feed, Addax can list it (crypto, gold, equities, and more).

## Anatomy of a trade

| Term | Meaning |
|---|---|
| **Collateral / margin** | The amount you put up (USDC, ADDX, or zkLTC) |
| **Leverage** | Multiplier applied to your collateral, 1x–100x |
| **Position size** | `collateral x leverage`, in USD notional |
| **Direction** | Long (price up) or Short (price down) |
| **Entry price** | Oracle mark price at execution, adjusted by spread |
| **Liquidation price** | Price at which losses consume your margin |
| **Take-profit / stop-loss** | Optional automatic close triggers |

## Order flow

1. **Open**: submit a market or limit order with your collateral, leverage, direction, and optional TP/SL.
2. **Price**: the oracle mark price is fetched; market orders execute immediately, limit orders wait for your target.
3. **Manage**: adjust TP/SL, or partially/fully close at any time.
4. **Close / trigger**: you close manually, or a keeper closes your position when TP, SL, or liquidation conditions are met.

## What moves your PnL

- **Price movement** relative to your entry, multiplied by leverage.
- **Spread** applied at open and close.
- **Borrowing/holding fees** accrued over time for keeping a position open (see [Fees & Spread](fees-and-spread.md)).

Next: [Opening & Closing Trades](opening-closing-trades.md).
