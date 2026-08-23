# Trading Contracts

This page shows how to interact with the Addax trading stack directly. Addresses are on the [Contracts & Addresses](../protocol/contracts.md) page; examples target the **gUSDC stack**.

## Contract roles

| Contract | Use it to |
|---|---|
| **Trading** | Open trades, close, update TP/SL, place/cancel limit orders, `executeNftOrder` (keepers) |
| **Trading Storage** | Read open trades, open limit orders, per-pair config |
| **Pair Infos** | Read spread, fees, borrowing rates, leverage caps |
| **Price Aggregator** | `fulfillOrder` (keepers), read price request state |
| **Vault (gToken)** | Deposit/withdraw liquidity (ERC-4626-style) |

## Opening a market trade

`openTrade` takes a trade struct, an order type, a spread-reduction id, slippage, and a referrer. For a market order the trade opens and settles in the same transaction (self-fulfilling).

```typescript
import { createWalletClient, http } from "viem";
import { privateKeyToAccount } from "viem/accounts";

// Trade tuple (gTrade v6 layout):
// trader, pairIndex, index, initialPosToken, positionSizeCollateral,
// openPrice, buy, leverage, tp, sl
const trade = {
 trader: account.address,
 pairIndex: 0n, // BTC, see Pair List
 index: 0n,
 initialPosToken: 0n,
 positionSizeDai: 100_000000n, // 100 USDC (6 decimals)
 openPrice: markPrice, // 1e10-scaled price
 buy: true, // long
 leverage: 50n, // 50x
 tp: 0n, // optional take-profit price
 sl: 0n, // optional stop-loss price
};

// orderType: 0 = MARKET, 1 = LIMIT (open)
await trading.write.openTrade([trade, orderType, spreadReductionId, slippageP, referrer]);
```

> Confirm the exact struct field order and price scaling against the deployed ABI before sending funds, layouts differ slightly between gTrade versions.

## Placing & canceling a limit order

Open a limit by calling `openTrade` with the LIMIT order type, the order is stored on-chain until a keeper executes it. Cancel it with:

```typescript
await trading.write.cancelOpenLimitOrder([pairIndex, index]);
```

Check for an existing open limit order via Storage:

```typescript
const has = await storage.read.hasOpenLimitOrder([trader, pairIndex, index]);
const order = await storage.read.getOpenLimitOrder([trader, pairIndex, index]);
```

## Closing / updating

```typescript
// Close a market position (partial or full)
await trading.write.closeTradeMarket([pairIndex, index]);

// Update take-profit / stop-loss on a live position
await trading.write.updateTp([pairIndex, index, newTpPrice]);
await trading.write.updateSl([pairIndex, index, newSlPrice]);
```

## Keeper execution (triggers)

Limit, TP, SL, and liquidation are executed in two steps by keepers:

```typescript
// 1. Initiate the pending NFT order on Trading
// orderType: TP / SL / LIQ / OPEN
await trading.write.executeNftOrder([orderType, trader, pairIndex, index, nftId, nftType]);

// 2. Resolve the price on the aggregator to complete it
await priceAggregator.write.fulfillOrder([orderId, priceData]);
```

See [Building Keeper Bots](building-bots.md) for a full, robust implementation, and [Keepers](../protocol/keepers.md) for the conceptual flow.

## ABIs

The app and keepers keep trimmed ABIs in the repo:

- Frontend: `lib/perps/abi.ts`
- Keepers: `perps-keepers/src/lib/abis.ts`

These expose the exact function and event fragments Addax uses (`openTrade`, `cancelOpenLimitOrder`, `executeNftOrder`, `fulfillOrder`, `hasOpenLimitOrder`, `getOpenLimitOrder`, plus lifecycle events like `NftOrderInitiated`, `OpenLimitPlaced/Updated/Canceled`, and callback events).
