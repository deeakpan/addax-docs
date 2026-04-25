# Pools

A pool is a smart contract that holds two tokens and prices them against each other using a concentrated liquidity model. Every swap on Addax goes through one or more pools.

## How a pool is identified

Every pool is uniquely identified by three values: `token0`, `token1`, and `fee`. The same token pair can have multiple pools at different fee tiers. `token0` is always the lexicographically lower address.

```
Pool = (token0, token1, fee)
```

## Price and ticks

Price in a V3 pool is represented as `sqrtPriceX96` — the square root of the price scaled by 2^96. Instead of a continuous price curve, liquidity is allocated in discrete price buckets called **ticks**.

Each tick represents a 0.01% price step. LPs choose a lower and upper tick to define their active range. When the pool price is inside that range, the position earns fees. When it moves outside, the position stops earning until price returns.

## Creating a pool

Anyone can create a pool permissionlessly via the factory:

```solidity
address pool = AddaxV3Factory.createPool(
    0x6eF676c26E8C977554DA186eD0B215956E8F8753, // wzkLTC
    0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50, // aUSDC
    3000 // 0.3% fee tier
);
```

The pool starts with zero liquidity and no price. The first LP to mint a position sets the initial price by choosing `sqrtPriceX96`.

## Fee tiers

| Fee | Pool param | Best for |
|---|---|---|
| 0.05% | `500` | Stable or pegged pairs |
| 0.3% | `3000` | Most standard pairs |
| 1% | `10000` | Exotic or volatile pairs |

## What determines execution quality

- **Depth** — More liquidity in the active range means lower price impact per trade.
- **Range width** — Tighter ranges are more capital efficient but go out of range more often.
- **Fee tier** — Lower fee pools attract more volume; higher fee pools compensate LPs for volatile pairs.
