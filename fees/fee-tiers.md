# Fee Tiers

Every Addax V3 pool is created with a fixed fee tier. The fee is charged on each swap and distributed to liquidity providers in that pool.

## Available tiers

| Tier | Best for |
|---|---|
| 0.05% (500) | Stable pairs or highly correlated assets |
| 0.3% (3000) | Most standard pairs (e.g. wzkLTC/USDC) |
| 1% (10000) | Exotic or low-liquidity pairs |

## How the tier is set

The fee tier is chosen when a pool is created via `AddaxV3Factory.createPool(tokenA, tokenB, fee)`. It cannot be changed after creation. If you want a different fee tier for the same pair, a separate pool must be created.

## Choosing the right tier

A lower fee tier is not always better for traders. What matters is net output:

- A 0.05% pool with thin liquidity causes higher price impact than a 0.3% pool with deep liquidity.
- The aggregator compares net output across all fee tiers and routes to the best one automatically.
