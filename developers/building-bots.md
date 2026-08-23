# Building Keeper Bots

Addax relies on keepers to keep prices fresh and to execute triggers that cannot self-fulfill: limit opens, take-profit, stop-loss, and liquidations. Anyone can run a keeper. Successful triggers earn a reward.

## Start here: `addax_protocol/bots`

Use the reference bots in the **`addax_protocol/bots`** repository. Clone that repo, follow its setup guide, and configure the required environment variables there. Keep keys and operator secrets out of public channels.

At a high level you will need:

- A **LitVM RPC** endpoint (HTTP, and ideally WebSocket for live event subscriptions).
- A **funded keeper wallet** with enough native gas token to submit transactions continuously.
- The ability to **sign and send contract calls** as that wallet (private key or equivalent signer managed securely).
- Access to the **keeper NFT** (or equivalent authorization) required by the trading contracts to initiate trigger orders.
- Whatever **state/indexing** configuration the bots repo expects so your bot can discover open positions and pending limits efficiently.

Exact variable names, ABIs, and helper modules live in `addax_protocol/bots`. Treat that repo as the source of truth.

## What keepers do

### Trigger keeper

Scans open positions and pending limit orders against the live oracle mark and executes anything due:

1. Detect OPEN / TP / SL / LIQ conditions.
2. Initiate the trigger on the **Trading** contract.
3. Fulfill the price on the **Price Aggregator** so callbacks can settle the trade.
4. Prefer simulating first and skipping known reverts (timelock, condition no longer valid, already executed).

### Oracle / price keeper

Keeps the on-chain price feed fresh so fulfillments always have recent data to settle against.

## Requirements for operators

- **Gas**: keepers submit many transactions. Keep the wallet topped up with LitVM gas token.
- **Signing**: the bot must sign Trading and Price Aggregator calls. Protect the key; use a dedicated operator wallet.
- **Authorization**: the signing wallet must hold the required keeper NFT (or be otherwise authorized) for trigger initiation.
- **Uptime**: prefer WebSocket subscriptions for live events; fall back to polling only if needed.
- **Rewards**: successful permissionless executions earn the configured keeper reward.

## Conceptual loop

```typescript
for (const position of openPositions) {
  const mark = await readOracleMark(position.pairIndex);
  const kind = classifyTrigger(position, mark); // TP | SL | LIQ | null
  if (!kind) continue;

  // Simulate, then submit if valid
  await trading.executeNftOrder(/* orderType, trader, pair, index, nftId, ... */);
  await priceAggregator.fulfillOrder(/* orderId, pricePayload */);
}
```

For the protocol-level flow (two-step triggers, roles, rewards), see [Keepers](../protocol/keepers.md). For contract entry points, see [Trading Contracts](contracts.md).
