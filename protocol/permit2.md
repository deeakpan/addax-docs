# Permit2

Permit2 is the token approval and transfer contract that all ARIS reactors use to move tokens from users. Instead of approving each reactor contract individually, users approve Permit2 once and then sign typed messages that authorise specific transfers per order.

**ArisPermit2:** `0xE6c670CED4e06e088AEb7141E0A0a557da517DbB`

## How it works

1. User calls `token.approve(permit2Address, MaxUint256)` once per token.
2. Each order the user signs includes a `nonce` and `deadline`. The signature authorises Permit2 to transfer exactly `inputAmount` of the input token to the filler when the order is executed.
3. The reactor calls `permit2.permitWitnessTransferFrom(...)` during execution. Permit2 checks the signature, verifies the nonce has not been used, and transfers the tokens.
4. After execution the nonce is consumed and cannot be replayed.

## Nonce-based cancellation

Because each order commits to a unique nonce, you can cancel an order onchain by invalidating its nonce directly on Permit2 before a filler executes it. This is a harder, guaranteed cancellation compared to the API-level soft cancel.

```typescript
const PERMIT2_ABI = [
  "function invalidateNonces(address token, address spender, uint48 newNonce)",
];

const permit2 = new ethers.Contract(
  "0xE6c670CED4e06e088AEb7141E0A0a557da517DbB",
  PERMIT2_ABI,
  signer
);

// Invalidate all nonces up to and including the order's nonce
await permit2.invalidateNonces(tokenAddress, reactorAddress, orderNonce + 1);
```

## Allowance vs signature transfer

Permit2 supports two modes:

**Allowance transfer** — set a blanket allowance for a spender up to an amount and expiry. Used for recurring approvals.

**Signature transfer** — single-use transfer authorised by a typed signature. This is what ARIS reactors use. Each order fill is a fresh signature, so there is no standing allowance that could be exploited.
