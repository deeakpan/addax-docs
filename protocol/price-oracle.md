# Price Oracle

Addax is an oracle-priced protocol: every open, close, and trigger settles at a price resolved from an on-chain oracle rather than from an order book. This page explains where prices come from and how they enter the trading contracts.

## Sources

| Piece | Address / key | Role |
|---|---|---|
| **Price Aggregator** | `0x3f0fA5CeC8B45111777baB68E37155ca0edC6400` (gUSDC stack) | Resolves prices and `fulfillOrder`s pending requests |
| **DIA Oracle** | `0xFf856a958eFA7965A4dFC2BFb09dDbc9EABe9aAb` | Underlying price feed |
| **Oracle keys** | e.g. `BTC/USD`, `ETH/USD`, `XAU/USD` | Per-pair feed identifiers |

Each market maps to an oracle key (see the [Pair List](../trading/pair-list.md)). The aggregator reads the DIA feed for that key when a price is requested.

## How pricing flows into a trade

1. **Market orders** request a price and settle in the same transaction — the aggregator returns the current mark price and Callbacks applies it immediately.
2. **Limit / TP / SL / LIQ** orders are initiated by a keeper (`executeNftOrder`), then a `fulfillOrder` call on the aggregator resolves the price and completes the action.

## Mark price and spread

The oracle provides the **mark price**. On top of it, each pair applies a **spread** (and price impact for large size) so that longs open slightly above and shorts slightly below mark. This spread compensates the vault and is configured per pair in **Pair Infos**. See [Fees & Spread](../trading/fees-and-spread.md).

## The oracle keeper

Feed freshness is maintained by an **oracle keeper** — an off-chain process that pushes updated prices on a schedule so the aggregator always has recent data to fulfill against. See [Keepers](keepers.md) for how the oracle keeper and trigger keeper run.
