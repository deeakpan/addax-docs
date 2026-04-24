# Aggregation and Best Price

Addax routes swaps through its own pools and external DEX liquidity on LitVM, finding the best price across all available venues in a single transaction.

## AddaxAggregatorV3

The `AddaxAggregatorV3` contract (`0x7dB9ac619cE9A82ffd9c26fbb2Dc5BeD0215E2d6`) is the onchain aggregation layer. It accepts any allowlisted router address per swap call, enabling routing across multiple DEXes without redeploying.

**Supported swap types:**
- Exact input single-hop
- Exact input multi-hop (path routing)
- Exact output single-hop
- Exact output multi-hop
- Multicall — batch multiple swaps in one transaction

## Integrated DEXes on LitVM

Addax aggregation currently routes across the following liquidity venues on LitVM LiteForge:

| DEX | Link |
|---|---|
| Onmi Fun | [app.onmi.fun](https://app.onmi.fun/) |
| Lester Labs | [lester-labs.com](https://www.lester-labs.com/) |

New venues can be added by the Addax team via `addRouter` on the aggregator contract — no redeployment required.

## What "best price" means

Best price is not only spot price. It accounts for:

- Route output amount
- Fee tier costs
- Price impact from liquidity depth
- Multi-hop opportunities across venues

## How it works

1. Quotes are gathered from all integrated routers for the requested pair.
2. Output amounts are normalized and compared.
3. The highest net output within the user's slippage tolerance is selected.
4. The swap is routed through the winning venue via the aggregator in one transaction.

## Why this helps

- Better fills for size — deeper combined liquidity across venues.
- Fewer failed swaps from thin individual pools.
- More robust execution in volatile conditions.
