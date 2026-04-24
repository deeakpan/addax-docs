# LP Basics & How You Earn

When you provide liquidity on Addax, you deposit two tokens into a pool within a chosen price range. Every swap that passes through your active range generates a fee, and you earn a share of it proportional to your liquidity.

## How fee earnings work

When a swap moves through the pool:
1. The pool charges the swap fee (e.g. 0.3%) on the input amount.
2. The fee is distributed among all positions whose range covers the current tick.
3. Your share = your liquidity at that tick / total liquidity at that tick.

**Example:** The wzkLTC/USDC 0.3% pool processes a 1,000 USDC swap. The fee is 3 USDC. If you hold 25% of the active liquidity at that tick, you earn 0.75 USDC.

Fees accumulate inside your NFT position. They do not compound automatically — you call `collect()` on the NonfungiblePositionManager to withdraw them.

## Impermanent loss

As price moves, the pool rebalances your holdings. If wzkLTC price rises, the pool converts more of your wzkLTC into USDC as traders buy. If you withdrew at a higher price than you entered, you would have been better off just holding — that difference is impermanent loss.

The narrower your range, the more pronounced this rebalancing is. Wider ranges behave more like a traditional AMM with less rebalancing but also less fee concentration.

## Narrow vs wide ranges

| Range | Fee efficiency | Rebalance risk | Maintenance |
|---|---|---|---|
| Tight (±5%) | High — earns more per unit | High — goes out of range often | Active |
| Wide (±50%) | Lower | Low | Passive |
| Full range | Lowest — like a V2 pool | Minimal | None |

## Out-of-range positions

When the market price moves outside your tick range, your position holds 100% of one token and earns zero fees until price returns. This is not a loss — you still own the tokens — but you are not earning.
