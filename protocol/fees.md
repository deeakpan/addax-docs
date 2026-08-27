# Protocol Fees

This page describes how fees are structured and routed on-chain. For a trader-focused explanation of how fees affect your PnL, see [Fees & Spread](../trading/fees-and-spread.md).

## Fee types

| Fee | Charged on | When |
|---|---|---|
| **Spread** | Mark price | Open and close |
| **Opening fee** | Position notional | On open |
| **Closing fee** | Position notional | On close |
| **Price impact** | Position notional | On open/close, scales with size & skew |
| **Funding** | Position notional | Continuously while open (only if long/short OI is skewed) |

All per-pair fee parameters (spread, fee percentages, funding rates, leverage caps) live in the **Pair Infos** contract for each stack and can be read on-chain.

## Where fees go

Opening and closing fees are split between:

- **gToken vault**: accrues to liquidity providers as increased assets per share.
- **Protocol / treasury**: protocol-controlled allocation.
- **Keeper / trigger rewards**: pays the keepers that execute limit, TP, SL, and liquidation orders.

**Funding** is peer-to-peer between longs and shorts (crowded side → lighter side). It does not go to the protocol. Net trader losses still settle against the vault, which is the counterparty to all trades.

Trader-facing funding explainer: [Funding Rates](../trading/funding-rates.md).

## Reading fees on-chain

Fee and pair parameters are exposed by the **Pair Infos** contract of each stack (gUSDC / gADDX / gzKLTC). See [Contracts & Addresses](contracts.md) for addresses and [Trading Contracts](../developers/contracts.md) for how to query them.
