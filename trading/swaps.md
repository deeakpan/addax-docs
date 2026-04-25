# Swaps

Every swap on Addax routes through the `AddaxV3SwapRouter` (`0xc6B464b146B1C9D03646b624f6B3ECA9794FdfB5`). The router finds the best path across available pools at each fee tier and executes the trade in a single transaction.

## Swap types

**Exact input** — you specify how much you are selling. You receive however much the pool gives at current price, subject to your slippage setting.

**Exact output** — you specify how much you want to receive. The router charges the minimum input required, with any excess refunded.

## Slippage

Slippage tolerance sets the worst acceptable price. If price moves beyond your tolerance between submission and execution, the transaction reverts. A tighter tolerance protects you but risks more reverts during volatile periods.

## Price impact

Large trades relative to pool depth move the price. The router shows estimated price impact before you confirm. High impact trades are better split across multiple smaller swaps or routed through the aggregator across multiple venues.

## Multi-hop

When no direct pool exists for a pair, the router can route through an intermediate token. For example, if there is no direct TOKEN/USDC pool, the router can go TOKEN → wzkLTC → USDC across two pools in one transaction.
