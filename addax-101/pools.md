# Pools

Pools are smart contracts that hold two assets and quote prices based on liquidity and ticks.

## Pool basics

- Each pool is uniquely identified by `token0`, `token1`, and `fee tier`.
- Common fee tiers include `0.05%`, `0.3%`, and `1%`.
- Depth and range distribution determine execution quality.

## Why pool quality matters

- Better depth means lower price impact.
- Wider active liquidity means better execution over more price movement.
- Fragmented liquidity can force more expensive routes.

## On Addax

- Pool discovery is surfaced in the Pools page.
- Pool TVL and fee tier are exposed for quick filtering.
- LPs can mint concentrated positions via NFT positions.

