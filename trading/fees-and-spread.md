# Fees & Spread

Addax has a simple, transparent fee model. Because trades settle against a vault at the oracle price, there is no order-book slippage — your costs are the spread, an opening/closing fee, price impact, and time-based holding fees.

## Spread

Each market has a **spread** applied symmetrically around the oracle mark price. Longs open slightly above and shorts slightly below the mid, and vice-versa on close. Spread compensates the vault for taking the other side and varies by market volatility and liquidity.

## Opening & closing fee

A percentage fee on **position size** (notional), charged when you open and when you close. This fee funds:

- the **gToken vault** (liquidity providers),
- protocol/treasury allocation,
- **keeper/trigger rewards** for executing orders.

Because it's charged on notional (collateral × leverage), higher leverage means a higher absolute fee for the same collateral.

## Price impact

Large positions relative to the market's open interest incur **price impact** — a small adjustment to entry/exit price that scales with size and existing skew. This keeps long/short open interest balanced and protects the vault.

## Borrowing / holding fees

Open positions accrue a **borrowing (holding) fee** over time, based on the market's open-interest skew. If you're on the crowded side of the market you pay more; this incentivizes balanced open interest and compensates the vault for the risk it carries. Holding fees continuously erode margin, so they also move your liquidation price against you.

## Putting it together

Your net PnL when you close is roughly:

```
PnL = (exit − entry) × direction × size
      − openFee − closeFee
      − accruedHoldingFees
      ± priceImpact
```

For the on-chain fee parameters and where each fee is routed, see [Protocol Fees](../protocol/fees.md).
