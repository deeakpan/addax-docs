# Architecture Overview

Addax is a leveraged trading protocol built on the gTrade / Gains Network contract architecture, deployed on LitVM. This page describes the on-chain components, how they interact, and the external infrastructure Addax leans on.

## Infrastructure partners

Addax is intentionally built on proven, widely adopted providers rather than bespoke systems for pricing and indexed history.

| Partner | Role |
|---|---|
| **[DIA](https://www.diadata.org/)** | Market data oracle: mark prices for settlement |
| **[Goldsky](https://goldsky.com/)** | Subgraph indexing: reliable access to on-chain trading activity |

<p align="left">
 <a href="https://www.diadata.org/"><img src="https://avatars.githubusercontent.com/u/42144424?s=64&v=4" alt="DIA" width="40" height="40" /></a>
 &nbsp;&nbsp;
 <a href="https://goldsky.com/"><img src="https://avatars.githubusercontent.com/u/90223190?s=64&v=4" alt="Goldsky" width="40" height="40" /></a>
</p>

[DIA](https://www.diadata.org/) | [DIA brand assets](https://www.diadata.org/brand-assets/) | [Goldsky](https://goldsky.com/) | [Goldsky brand kit](https://goldsky.link/brand-kit)

**Oracles (DIA).** Entries, exits, and liquidations settle against DIA marks. Testnet uses push feeds with a **0.1%** deviation threshold; mainnet is planned around DIA **pull** reports so each trade can consume a freshly signed price. Details: [Price Oracle](price-oracle.md).

**Indexing (Goldsky).** Protocol events (opens, closes, liquidations, limit lifecycle) are indexed through Goldsky subgraphs. That gives applications, keepers, and analytics a standard GraphQL surface over trading history without repeatedly scanning the full chain. Goldsky is a common choice for production subgraph hosting across DeFi.

## Component map

| Component | Role |
|---|---|
| **Trading** | User entry point: open/close, update TP/SL, keeper trigger entry |
| **Trading Storage** | Canonical state: open trades, open limits, pair config, NFT/keeper bookkeeping |
| **Trading Callbacks** | Settlement: applies price results, computes PnL, emits lifecycle events |
| **Price Aggregator** | Resolves oracle prices and completes pending price requests |
| **DIA Oracle** | Underlying price source consumed by the aggregator |
| **Pair Infos** | Per-pair parameters: spread, fees, borrowing, leverage caps |
| **gToken Vault** | Collateral vault / counterparty; mints gToken LP shares |
| **Staking** | ADDX staking and reward distribution |

Each **collateral** (USDC, ADDX, WzkLTC) has its own stack, a full set of Trading / Storage / Callbacks / Vault / Pair Infos / Price Aggregator contracts. The gUSDC stack is the primary one; gADDX and gzKLTC mirror it.

## Trade lifecycle

### Market order (self-fulfilling)

1. Trader calls open (or close) on **Trading**.
2. In the same transaction, the stack resolves the oracle price and settles via **Callbacks**: the position opens or closes immediately.

### Limit / TP / SL / Liquidation (two-step, keeper-driven)

1. A **keeper** initiates the trigger on **Trading**.
2. A fulfill step on the **Price Aggregator** applies the oracle price and completes settlement through **Callbacks**.

This design lets anyone execute triggers permissionlessly and earn a reward, while keeping pricing anchored to the oracle. See [Keepers](keepers.md).

## Design lineage

Addax follows the gTrade v6-style model: oracle-priced synthetic trades, gToken collateral vaults as counterparty, NFT-gated keeper execution for triggers, and per-pair risk parameters. See [Trading Contracts](../developers/contracts.md) for function-level integration details.
