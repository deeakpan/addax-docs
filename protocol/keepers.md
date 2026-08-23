# Keepers

Keepers are off-chain bots that keep Addax live: they refresh prices and execute the orders that cannot self-fulfill (limits, take-profit, stop-loss, and liquidations). Execution is permissionless and rewarded, so the protocol does not depend on a single operator.

Addax has two keeper roles:

## 1. Trigger keeper

Scans open positions and pending limit orders against the live oracle mark and executes anything whose conditions are met:

- **OPEN**: a pending limit whose target price is reached.
- **TP**: a position that hit take-profit.
- **SL**: a position that hit stop-loss.
- **LIQ**: a position past its liquidation threshold.

For each match, the keeper runs a **two-step trigger**:

1. Initiate the order on the **Trading** contract (emits a pending NFT/order event).
2. Fulfill the price on the **Price Aggregator**, which settles the open or close through Callbacks.

Operators should simulate first and skip known reverts (timelock not elapsed, condition no longer valid, already executed). Successful triggers earn a reward.

## 2. Oracle keeper

Keeps the on-chain price feed fresh so the aggregator always has recent data when fulfilling triggers and market settlements.

## Running keepers

Reference implementations and setup live in **[addax_protocol/bots](https://github.com/addax_protocol/bots)** on GitHub. Start from that repository: configure RPC access, a gas-funded signing wallet, keeper authorization (NFT), and any indexing/state settings the bots expect.

You will need:

- LitVM RPC (HTTP, preferably with WebSocket).
- A dedicated wallet with enough gas to operate continuously.
- Ability to sign Trading and Price Aggregator transactions.
- The keeper NFT (or equivalent permission) required by the contracts.

See [Building Keeper Bots](../developers/building-bots.md) for operator guidance.
