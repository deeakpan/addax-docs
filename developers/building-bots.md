# Building Keeper Bots

Addax relies on keepers to push prices and execute triggers (limit opens, take-profit, stop-loss, liquidations). The reference implementations live in the repo under `perps-keepers` (trigger keeper) and `oracle-keeper` (price keeper). This page shows how to run and extend them.

## Trigger keeper

Scans open positions and pending limit orders against the live oracle price and executes anything due.

### Commands

```bash
# in perps-keepers/
npm start # long-running watcher (WebSocket subscriptions + continuous scan)
npm run once # single scan+execute pass, then exit (good for cron)
npm run db:init # create Supabase tables from .env
npm run db:reset # wipe and recreate keeper tables/cursors
```

### What it does each pass

1. Load open positions and pending limit orders (from the indexed mirror in Supabase).
2. For each, compare against the current oracle price to decide if OPEN / TP / SL / LIQ conditions are met.
3. Execute the **two-step trigger**:
 - `executeNftOrder(orderType, ...)` on Trading -> emits `NftOrderInitiated`.
 - `fulfillOrder` on the Price Aggregator -> resolves price and completes the action.
4. Simulate first and skip known reverts (timelock not elapsed, condition invalid, already executed), then submit.

### Environment

Key `.env` values (see `perps-keepers/.env.example`):

| Var | Purpose |
|---|---|
| `PERPS_RPC_URL` | LitVM RPC (HTTP) |
| `PERPS_SYNC_MODE` | `websocket` (recommended) or `poll` |
| `PERPS_POLL_INTERVAL_MS` | Scan interval when polling |
| `KEEPER_PRIVATE_KEY` | Wallet that owns the keeper NFT and pays gas |
| `SUPABASE_URL` / `SUPABASE_SERVICE_ROLE_KEY` | State mirror |
| `SUPABASE_DB_URL` | Direct Postgres URL for schema management |

> The keeper wallet must hold the keeper NFT used by `executeNftOrder`. On this deployment the trigger NFT is minted to the deployer, so use that key (or transfer the NFT).

### Reliability notes

- Prefer **WebSocket** mode: `eth_subscribe` avoids slow `eth_getLogs` polling.
- Startup log catch-up runs **best-effort and non-blocking**, so the live subscription comes up immediately even if historical `getLogs` is slow.
- RPC calls use extended timeouts and retries.

## Oracle keeper

Keeps the price feed fresh so the aggregator always has recent data to fulfill against.

```bash
# in oracle-keeper/
npm start # long-running watch loop, pushes price updates on interval
npm run once # push a single update, then exit
```

Configure `ORACLE_POLL_INTERVAL_MS` and the RPC/keeper credentials in `oracle-keeper/.env`.

## Writing your own keeper

Any wallet holding the keeper NFT can execute triggers permissionlessly and earn rewards. The minimal loop is:

```typescript
for (const pos of openPositions) {
 const price = await getOraclePrice(pos.pairIndex);
 const kind = classify(pos, price); // "TP" | "SL" | "LIQ" | null
 if (!kind) continue;

 // simulate to skip known reverts
 const ok = await simulate(() => trading.executeNftOrder(orderType(kind), ...));
 if (!ok) continue;

 await trading.executeNftOrder(orderType(kind), pos.trader, pos.pairIndex, pos.index, nftId, nftType);
 await priceAggregator.fulfillOrder(orderId, priceData);
}
```

Reuse the trimmed ABIs in `perps-keepers/src/lib/abis.ts` and the two-step helper in `perps-keepers/src/lib/nft-orders.ts` as a starting point.
