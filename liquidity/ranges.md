# Range Strategy

Your price range is the core decision when providing liquidity. It determines how much fee income you earn, how often you go out of range, and how your token balances shift as price moves.

## How ticks map to prices

Price in a V3 pool is discretized into ticks. Each tick represents a 0.01% price step. The relationship is:

```
price = 1.0001 ^ tick
```

For the wzkLTC/USDC pool where USDC is token1 and wzkLTC is token0:
- tick 0 → price = 1 USDC per wzkLTC
- tick 46054 → price ≈ 100 USDC per wzkLTC
- tick -46054 → price ≈ 0.01 USDC per wzkLTC

## Tick spacing

Each fee tier enforces a minimum tick spacing:

| Fee tier | Tick spacing |
|---|---|
| 0.05% (500) | 10 |
| 0.3% (3000) | 60 |
| 1% (10000) | 200 |

Your `tickLower` and `tickUpper` must be multiples of the pool's tick spacing.

## Strategy patterns

**Tight active range (±5–10%)**
Deploy capital around the current price. High fee earnings per dollar when in range. Requires monitoring and rebalancing as price drifts.

**Balanced wide range (±30–50%)**
Earns less per unit of liquidity but stays active across broader price moves. Good for pairs that oscillate within a known band.

**Full range**
Equivalent to a traditional V2 AMM position. Never goes out of range. Lowest fee concentration — you compete with liquidity spread across all prices.

**Layered positions (multiple NFTs)**
Create several positions at different ranges with different allocations. For example: 50% of capital in a tight range for maximum earnings, 50% in a wide range as a backstop that always earns something.

## Choosing your range

Ask: what price movement am I expecting?

- If you expect price to stay near current levels → tight range
- If you expect volatility but want to stay passive → wide range
- If you have no view and want set-and-forget → full range
