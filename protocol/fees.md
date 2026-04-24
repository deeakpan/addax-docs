# Fees

Addax has three distinct fee layers. Understanding each one matters for both traders and LPs.

## 1. Pool swap fees (LP fees)

Every pool has a fixed fee tier set at creation — `0.05%`, `0.3%`, or `1%`. When a swap routes through a pool, this fee is charged on the input amount and distributed to liquidity providers whose range was active during the swap.

**How LP fee earnings are calculated:**

LPs earn fees proportional to their share of liquidity within the active tick range at the time of the swap. If your position covers 20% of the liquidity at the current tick, you earn 20% of that swap's fee.

Example: A 100 USDC swap through a 0.3% pool generates 0.30 USDC in fees. If your position holds 20% of liquidity in range, you earn 0.06 USDC from that single swap.

Fees accumulate inside your NFT position and are collected by calling `collect()` on the NonfungiblePositionManager — they are not auto-compounded.

## 2. Protocol fees (ARIS orders)

The ARIS reactor contracts support an optional protocol fee controlled by a `feeController` address set by the Addax team. From `ProtocolFees.sol`:

```solidity
uint256 private constant MAX_FEE_BPS = 5; // 0.05% maximum
```

The protocol fee is capped at **0.05%** of order value. It is injected into the resolved order as an additional output sent to the fee recipient. If no `feeController` is configured, no protocol fee is taken.

Currently the fee controller is not set — **ARIS orders on Addax are fee-free**.

## 3. Filler economics

ARIS fillers are independent actors. They earn profit by filling orders at a price slightly better than the user's minimum — capturing the spread between current market price and the user's stated minimum output. The protocol does not dictate filler margins; it is a competitive market.

## Summary

| Fee type | Who pays | Who receives | Amount |
|---|---|---|---|
| Pool swap fee | Trader | LPs in active range | 0.05% / 0.3% / 1% |
| Protocol fee | Order filler | Addax protocol | 0% (up to 0.05% max) |
| Filler margin | Implicit (user gets min output) | Filler | Market-determined |
