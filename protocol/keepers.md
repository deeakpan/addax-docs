# Keepers

Keepers are off-chain bots that keep Addax running: they push fresh prices and execute the orders that can't self-fulfill (limits, take-profit, stop-loss, and liquidations). Keeper execution is permissionless and rewarded, so the system stays live without relying on any single operator.

Addax runs two keeper roles:

## 1. Trigger keeper

The trigger keeper scans open positions and pending limit orders against the live oracle price and executes anything whose conditions are met:

- **OPEN** — a pending limit order whose target price is reached.
- **TP** — a position that hit its take-profit.
- **SL** — a position that hit its stop-loss.
- **LIQ** — a position past its liquidation threshold.

For each, the keeper performs the **two-step trigger**:

1. `executeNftOrder(orderType, …)` on the Trading contract — initiates the pending NFT order (emits `NftOrderInitiated`).
2. `fulfillOrder` on the Price Aggregator — resolves the price and completes the open/close via Callbacks.

The keeper simulates the transaction first and skips known revert reasons (e.g. timelock not elapsed, condition no longer valid), then submits. Successful triggers earn the keeper a reward.

### Running it

The keeper lives in `perps-keepers`:

- `npm start` — long-running watcher. Subscribes to live events over WebSocket, catches up missed logs on startup (best-effort, non-blocking), and continuously scans + executes triggers.
- `npm run once` — single pass: scan once, execute what's due, exit. Useful for cron-style runs.
- `npm run db:init` — create the keeper's Supabase tables from your `.env`.
- `npm run db:reset` — wipe and recreate keeper tables/cursors.

## 2. Oracle keeper

The oracle keeper keeps the price feed fresh so the aggregator always has recent data to fulfill against. It lives in `oracle-keeper`:

- `npm start` — long-running watch loop that pushes price updates on the configured interval.
- `npm run once` — push a single update and exit.

## State & indexing

The trigger keeper mirrors on-chain state (open positions, pending limit orders, lifecycle events) into Supabase for fast scanning and for the app's UI reads. It prefers **WebSocket subscriptions** for live events to avoid slow `eth_getLogs` polling, and treats startup log catch-up as best-effort so the live subscription comes up immediately.

For code-level details on running and extending keepers, see [Building Keeper Bots](../developers/building-bots.md).
