# Architecture Overview

Addax is a leveraged trading protocol built on the gTrade / Gains Network contract architecture, deployed on LitVM LiteForge. This page describes the on-chain components and how they interact.

## Component map

| Component | Role |
|---|---|
| **Trading** | User entry point — `openTrade`, close, update TP/SL, `executeNftOrder` for triggers |
| **Trading Storage** | Canonical state — open trades, open limit orders, per-pair config, NFT/keeper bookkeeping |
| **Trading Callbacks** | Settlement logic — applies price results, computes PnL, emits lifecycle events |
| **Price Aggregator** | Resolves oracle prices and `fulfillOrder`s pending requests |
| **DIA Oracle** | Underlying price feed source consumed by the aggregator |
| **Pair Infos** | Per-pair parameters: spread, fees, borrowing rates, leverage caps |
| **gToken Vault** | Collateral vault / counterparty; mints gToken LP shares |
| **Staking** | ADDX staking and reward distribution |

Each **collateral** (USDC, ADDX, WzkLTC) has its own stack — a full set of Trading / Storage / Callbacks / Vault / Pair Infos / Price Aggregator contracts. The gUSDC stack is the primary one; gADDX and gzKLTC mirror it.

## Trade lifecycle

### Market order (self-fulfilling)

1. Trader calls `openTrade` (or a close) on **Trading**.
2. In the same transaction, the contract resolves the oracle price and settles via **Callbacks** — the position opens/closes immediately. No separate keeper fulfill step is required.

### Limit / TP / SL / Liquidation (two-step, keeper-driven)

1. A **keeper** calls `executeNftOrder(orderType, …)` on **Trading**, which initiates a pending NFT order and emits `NftOrderInitiated`.
2. A `fulfillOrder` call on the **Price Aggregator** resolves the price and completes the open (limit) or close (TP/SL/LIQ) through **Callbacks**.

This two-step design lets anyone execute triggers permissionlessly and earn a reward, while keeping pricing anchored to the oracle. See [Keepers](keepers.md).

## Data & indexing

Position and order state lives on-chain in **Trading Storage**. The Addax app and keepers index lifecycle events (opens, closes, limit placements/cancellations, executions, liquidations) — over WebSocket subscriptions for live updates and via log catch-up on startup — and persist a mirror for fast UI reads.

## Design lineage

Addax follows the gTrade v6-style model: oracle-priced synthetic trades, gToken collateral vaults as counterparty, NFT-gated keeper execution for triggers, and per-pair risk parameters. See [Trading Contracts](../developers/contracts.md) for function-level integration details.
