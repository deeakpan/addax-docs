# Aggregation and Best Price

The Addax aggregator routes swaps across multiple DEX liquidity venues on LitVM, finding the best net output in a single transaction.

## How best price is determined

The aggregator queries all integrated routers for the requested pair and selects the route with the highest net output after accounting for fee tier costs and price impact. It does not simply pick the lowest fee — a 0.3% pool with deep liquidity often beats a 0.05% pool with thin liquidity.

## Integrated venues

| DEX | Link |
|---|---|
| Addax V3 | [addax.finance](https://addax.finance) |
| Onmi Fun | [app.onmi.fun](https://app.onmi.fun/) |
| Lester Labs | [lester-labs.com](https://www.lester-labs.com/) |

## Why this matters

Combining liquidity across venues means larger trades get better fills. A single pool may only have enough depth for a small trade before price impact becomes significant. Routing part of the trade through a second venue reduces that impact and improves the overall rate you receive.
