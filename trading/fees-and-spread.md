# Fees & Spread

Addax has a simple, transparent fee model. Because trades settle against a vault at the oracle price, there is no order-book slippage. Your costs are the spread, an opening/closing fee, price impact, and funding while a position is open.

## Spread

Each market has a **spread** applied symmetrically around the oracle mark price. Longs open slightly above and shorts slightly below the mid, and vice-versa on close. Spread compensates the vault for taking the other side and varies by market volatility and liquidity.

## Opening & closing fee

A percentage fee on **position size** (notional), charged when you open and when you close. This fee funds:

- the **gToken vault** (liquidity providers),
- protocol/treasury allocation,
- **keeper/trigger rewards** for executing orders.

Because it's charged on notional (collateral x leverage), higher leverage means a higher absolute fee for the same collateral.

## Price impact

Large positions relative to the market's open interest incur **price impact**: a small adjustment to entry/exit price that scales with size and existing skew. This keeps long/short open interest balanced and protects the vault.

## Funding

While a position is open, **funding** can transfer value between longs and shorts when one side is larger than the other. The crowded side pays; the lighter side earns. If the market is balanced, funding is near zero. Funding accrues into the position (it is not a separate hourly wallet payment) and can move your liquidation price over time.

See [Funding Rates](funding-rates.md) for the trader-facing explanation.

## Putting it together

Your net PnL when you close is roughly:

```
PnL = (exit − entry) x direction x size
 − openFee − closeFee
 ± funding
 ± priceImpact
```

For the on-chain fee parameters and where each fee is routed, see [Protocol Fees](../protocol/fees.md).
