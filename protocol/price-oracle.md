# Price Oracle

Addax settles every open, close, and trigger against an **on-chain oracle mark**, not an order book. Pricing is provided by **DIA**: a widely used oracle network that delivers transparent, multi-source market data to smart contracts.

<p align="left">
  <img src="https://avatars.githubusercontent.com/u/42144424?s=120&v=4" alt="DIA" width="48" height="48" />
</p>

## Why DIA

Rather than operating a custom price pipeline as the long-term source of truth, Addax relies on an established oracle provider. DIA aggregates first-party and exchange data and publishes feeds that trading contracts can read (or consume via signed updates) with clear freshness and integrity guarantees.

## Testnet: push feeds (0.1% deviation)

On **LitVM testnet**, markets are driven by DIA **push** feeds:

| Parameter | Value |
|---|---|
| Model | Push (on-chain storage updated by DIA) |
| Deviation threshold | **0.1%** |
| Heartbeat | Periodic forced refresh (alongside deviation) |
| Consumer API | Contracts read the latest published value (e.g. `getValue`) |

A push feed updates when the aggregated price moves by at least the deviation threshold, or when the heartbeat interval elapses. Between updates, the last written price is what settlement uses, subject to protocol-side staleness checks.

This configuration matches a common pattern among perpetual protocols that prefer simple on-chain reads during testnet and early production while still keeping marks reasonably tight.

## Mainnet: pull feeds

On **mainnet**, Addax plans to use DIA **pull** oracles:

- Prices are signed off-chain by the oracle network.
- A fresh report is submitted and verified **in the same transaction** as the trade action (open, close, or liquidation).
- Staleness and signer quorum are enforced on-chain at consumption time.

Pull delivery prioritizes mark freshness at the moment of execution, the usual requirement for leveraged markets at scale.

## How prices enter a trade

1. **Market orders** request a mark and settle in the same transaction once a valid price is available.
2. **Limit / TP / SL / liquidation** flows are started on-chain, then completed when a valid oracle price is applied (via the price aggregator / callbacks path).

The oracle supplies the **mark**. Each pair may still apply a **spread** (and size-based impact) so longs open slightly above mark and shorts slightly below. See [Fees & Spread](../trading/fees-and-spread.md).

## Feed registry (testnet)

| Role | Address |
|---|---|
| DIA / Addax price feed (testnet) | `0xFf856a958eFA7965A4dFC2BFb09dDbc9EABe9aAb` |
| Price aggregator (gUSDC) | `0xA184242a075bEA7012Ce83BD86f3E56a9bc33A73` |

Per-market keys and Chainlink-style feed adapters are listed with the deployment in [Contracts & Addresses](contracts.md).

## Related

- [Architecture Overview](overview.md), how the aggregator sits in the stack
- [Keepers](keepers.md), who drives triggers once a price is available
- [Fetching Prices](../developers/fetching-prices.md), reading marks for integrations
