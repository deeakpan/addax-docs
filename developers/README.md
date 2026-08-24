# Integration Overview

Build on Addax through three layers: **contract addresses** for direct on-chain integration, a **Public REST API** for read-only market and account data, and the **Trading API** for programmatic position management with prepared transactions your wallet signs locally.

Addax is non-custodial. Neither API holds private keys. The Trading API returns unsigned transaction payloads; your backend or client signs and broadcasts them.

## Choose your integration path

| Path | Best for | Auth | Rate limit |
|---|---|---|---|
| [Trading API](./trading-api.md) | Apps, bots, and desks opening/closing positions | API key (`x-addax-api-key`) | 300 req/min per key |
| [Public REST API](./public-api.md) | Dashboards, widgets, price displays | None | 60 req/min per IP |
| [Direct contracts](./contracts.md) | Full control, keepers, custom flows | On-chain wallet | N/A (RPC limits apply) |

Need higher throughput, dedicated infrastructure, or white-label access? Email **[contact@addax.finance](mailto:contact@addax.finance)** for Trading API keys and enterprise limits.

## Quick references

| What | Value |
|---|---|
| Chain ID | `4441` (LitVM) |
| RPC (HTTP) | `https://liteforge.rpc.caldera.xyz/http` |
| RPC (WS) | `wss://liteforge.rpc.caldera.xyz/ws` |
| Block explorer | `https://liteforge.explorer.caldera.xyz` |
| App / Public API base | `https://addax.finance` |
| Trading API base | `https://addax.finance/api/v1` |
| Contract addresses | [Contracts & Addresses](../protocol/contracts.md) |
| Oracle / DIA feeds | [Price Oracle](../protocol/price-oracle.md) |

## The gUSDC stack

Most integrations target the **gUSDC stack** (primary UI collateral):

| Contract | Role |
|---|---|
| **Trading** | `openTrade`, `closeTradeMarket`, TP/SL updates |
| **Storage** | Read open trades and limit orders |
| **Pair Infos** | Spread, fees, leverage caps |
| **Price Aggregator** | Oracle fulfillment path |
| **Vault (gUSDC)** | LP collateral vault |

Each collateral (USDC, ADDX, WzkLTC) has its own stack. Addresses are listed in [Contracts & Addresses](../protocol/contracts.md).

## Documentation map

- [Trading API](./trading-api.md) — prepare unsigned txs for open, close, TP/SL, limits
- [Public REST API](./public-api.md) — marks, positions, candles (rate-limited)
- [Direct Contract Integration](./contracts.md) — ABIs, structs, on-chain reads/writes
- [Fetching Prices](./fetching-prices.md) — DIA oracle keys and mark reads

Keeper operators: see [Keepers](../protocol/keepers.md) and the reference repo [addax-protocol/bots](https://github.com/addax-protocol/bots).
