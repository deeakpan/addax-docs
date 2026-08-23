# Order Types

Addax supports four order types. Market and limit orders **open** positions; take-profit and stop-loss orders **close** them. Liquidations are a fifth, protocol-enforced close trigger.

## Market order

Opens a position immediately at the current oracle mark price (adjusted by spread and price impact). Use this when you want to enter now.

- Executes in the same transaction as your submission.
- No waiting, subject only to the live price.

## Limit order

Opens a position only when the market reaches a price you specify. Use this to enter at a better level than the current price.

- Stored **on-chain** as a pending open order until conditions are met.
- Executed by a **keeper** via `executeNftOrder` (OPEN) + `fulfillOrder` when price crosses your target.
- Appears under **Open Orders / Limits**; can be **canceled** any time before it triggers.
- When you switch the trade panel to Limit mode, the price field pre-fills with the current mark price so you can nudge it to your target.

## Take-profit (TP)

An automatic close order that locks in profit once price reaches your target. Attach it when opening a trade or add it later to a live position.

- Optional, leave it blank and it simply won't trigger.
- Executed by a keeper when the mark price reaches the TP level.

## Stop-loss (SL)

An automatic close order that caps your loss once price moves against you to your chosen level.

- Optional but strongly recommended for leveraged positions.
- Executed by a keeper when the mark price reaches the SL level.
- Helps you exit before liquidation, preserving remaining margin.

## Liquidation (protocol trigger)

Not a user order, but the protocol's safety mechanism. If a position's losses consume its margin down to the maintenance threshold, a keeper liquidates it to protect the vault. See [Leverage & Liquidation](leverage-and-liquidation.md).

## How triggers execute on-chain

Limit, TP, SL, and liquidation are **two-step** on-chain actions:

1. A keeper calls `executeNftOrder(orderType, ...)` on the trading contract, which initiates a pending NFT order and emits `NftOrderInitiated`.
2. A `fulfillOrder` call on the price aggregator then resolves the price and completes the open or close.

Market orders are self-fulfilling, the open/close and price fulfillment happen in a single transaction. See [Keepers](../protocol/keepers.md) and [Trading Contracts](../developers/contracts.md) for details.
