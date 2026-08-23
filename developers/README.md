# Integration Overview

This section covers building on top of Addax, opening and managing trades programmatically, reading positions and prices, and running keeper bots.

## Sections

- [Trading Contracts](./contracts.md), interacting with the trading stack
- [Fetching Prices](./fetching-prices.md), reading mark/index prices and market stats
- [Building Keeper Bots](./building-bots.md), running the trigger and oracle keepers

## Quick references

| What | Where |
|---|---|
| Chain ID | `4441` |
| RPC (HTTP) | `https://liteforge.rpc.caldera.xyz/http` |
| RPC (WS) | `wss://liteforge.rpc.caldera.xyz/ws` |
| Block explorer | `https://liteforge.explorer.caldera.xyz` |
| Contract addresses | [Contracts & Addresses](../protocol/contracts.md) |
| REST API base | `https://addax.finance` |
| REST endpoints | `/api/perp/positions`, `/api/perp/open-limits`, `/api/perp/market-stats`, `/api/perp/vault-mark`, `/api/perp/trades`, `/api/perp/candles` |

## The stacks

Each collateral (USDC, ADDX, WzkLTC) has its own set of contracts. For most integrations, target the **gUSDC stack**: it is the primary UI and keeper stack. Read state from **Storage**, write via **Trading**, and read fee/leverage params from **Pair Infos**.
