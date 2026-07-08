# Leverage & Liquidation

## Leverage

Leverage multiplies your exposure relative to the collateral you post. Addax supports leverage from **1x up to 150x**, depending on the market.

```
position size (USD notional) = collateral × leverage
```

For example, 100 USDC at 50x gives a 5,000 USD position. A 1% move in your favor is a ~50% gain on your collateral; a 1% move against you is a ~50% loss. Higher leverage amplifies both PnL and liquidation risk.

Maximum leverage varies by asset class — crypto majors support the highest leverage, while equities and some commodities are capped lower. See the [Pair List](pair-list.md).

## Liquidation

A position is **liquidated** when its accumulated losses (plus fees) erode its margin down to the maintenance threshold. Liquidation protects the collateral vault from taking on undercollateralized risk.

### Liquidation price

The liquidation price is the market price at which your remaining margin is exhausted. As a simplified model:

- **Long:** `liqPrice ≈ entry × (1 − 1/leverage + feeBuffer)`
- **Short:** `liqPrice ≈ entry × (1 + 1/leverage − feeBuffer)`

Higher leverage pushes the liquidation price closer to your entry. Borrowing/holding fees accrued over time also move your liquidation price against you.

The exact liquidation price shown in the app accounts for spread, price impact, and accrued fees. Treat the formula above as an approximation.

### How liquidation happens

1. A keeper monitors open positions against the live oracle price.
2. When a position crosses its liquidation threshold, the keeper calls `executeNftOrder(LIQ, …)` followed by `fulfillOrder`.
3. The position is closed, remaining margin (if any) is returned, and the keeper earns a liquidation reward.

### Avoiding liquidation

- Use a **stop-loss** to exit before the liquidation price is reached.
- Use **lower leverage** to widen the buffer between entry and liquidation.
- **Add margin** or reduce size if a position moves against you.
- Watch **borrowing/holding fees** on long-held positions — they erode margin over time.

See [Fees & Spread](fees-and-spread.md) for how fees affect your position, and [Keepers](../protocol/keepers.md) for how triggers are executed.
