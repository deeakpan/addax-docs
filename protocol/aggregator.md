# Aggregator

The `AddaxAggregatorV3` contract (`0x7dB9ac619cE9A82ffd9c26fbb2Dc5BeD0215E2d6`) routes swaps across multiple DEX routers without the user needing to interact with each one individually.

## Integrated venues

| DEX | Link |
|---|---|
| Addax V3 | [addax.finance](https://addax.finance) |
| Onmi Fun | [app.onmi.fun](https://app.onmi.fun/) |
| Lester Labs | [lester-labs.com](https://www.lester-labs.com/) |

New routers are added by the Addax team via `addRouter(address)` — no redeployment required.

## Supported swap types

- `exactInputSingle` — swap exact amount of tokenIn for as much tokenOut as possible
- `exactInput` — same but multi-hop (path encoded as `tokenA → fee → tokenB → fee → tokenC`)
- `exactOutputSingle` — spend as little tokenIn as needed to receive exact tokenOut
- `exactOutput` — same but multi-hop
- `multicall` — batch any of the above in one transaction

## How it works

1. Caller approves the aggregator for `tokenIn`.
2. Caller calls the desired swap function, passing the target `router` address.
3. The aggregator pulls `tokenIn` from the caller, approves the router, and forwards the call.
4. Output lands on `params.recipient` directly from the router.
5. Approval is reset to 0 after the swap. For `exactOutput` variants, any unspent `tokenIn` is refunded.

## Multicall example

Batch a wrap and a swap in one transaction:

```typescript
const wrapCall = aggregator.interface.encodeFunctionData("wrapLegacy", [
  NEW_WZKLTC, amount
]);
const swapCall = aggregator.interface.encodeFunctionData("exactInputSingle", [
  ADDAX_ROUTER,
  { tokenIn: NEW_WZKLTC, tokenOut: USDC, fee: 3000, recipient, deadline, amountIn: amount, amountOutMinimum: 0, sqrtPriceLimitX96: 0 }
]);

await aggregator.multicall([wrapCall, swapCall]);
```
