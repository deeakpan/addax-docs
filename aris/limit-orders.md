# Limit Orders

A limit order fills when a filler can satisfy the output requirement — i.e., when market price is at or better than the user's specified price. The order stays open until filled, cancelled, or expired.

**Reactor:** `ArisLimitOrderReactor` — `0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502`

## How it works

The user specifies an exact `inputAmount` and a minimum `outputAmount`. The reactor transfers exactly `inputAmount` from the user via Permit2 and requires the filler to deliver at least `outputAmount` to the recipient. The filler keeps any surplus.

## Placing a limit order (TypeScript)

```typescript
import { ethers } from "ethers";

const LIMIT_REACTOR = "0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502";
const PERMIT2 = "0xE6c670CED4e06e088AEb7141E0A0a557da517DbB";
const CHAIN_ID = 4441;

// EIP-712 domain
const domain = {
  name: "Permit2",
  chainId: CHAIN_ID,
  verifyingContract: PERMIT2,
};

const order = {
  info: {
    reactor: LIMIT_REACTOR,
    swapper: await signer.getAddress(),
    nonce: Date.now(), // use a proper nonce manager in production
    deadline: Math.floor(Date.now() / 1000) + 3600, // 1 hour
    additionalValidationContract: ethers.ZeroAddress,
    additionalValidationData: "0x",
  },
  input: {
    token: WZKLTC_ADDRESS,
    amount: ethers.parseEther("1"),     // sell 1 wzkLTC
    maxAmount: ethers.parseEther("1"),
  },
  outputs: [
    {
      token: USDC_ADDRESS,
      amount: ethers.parseUnits("90", 6), // want at least 90 USDC
      recipient: await signer.getAddress(),
    },
  ],
};

const encodedOrder = ethers.AbiCoder.defaultAbiCoder().encode(
  ["tuple(tuple(address,address,uint256,uint256,address,bytes),tuple(address,uint256,uint256),tuple(address,uint256,address)[])"],
  [[order.info, order.input, order.outputs]]
);

// Sign with Permit2 witness (use @uniswap/permit2-sdk for production)
const signature = await signer.signTypedData(domain, types, orderData);

// Submit to API
await fetch("https://addax.finance/api/orders", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    orderHash: ethers.keccak256(encodedOrder),
    encodedOrder,
    signature,
    orderType: "Limit",
    offerer: await signer.getAddress(),
    expiresAt: new Date(order.info.deadline * 1000).toISOString(),
    chainId: CHAIN_ID,
  }),
});
```

## Cancelling a limit order

Orders can be cancelled by invalidating the Permit2 nonce onchain, or by calling the cancel API endpoint:

```typescript
await fetch("https://addax.finance/api/orders/cancel", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    orderHash: "0x...",
    chainId: 4441,
    reason: "user_cancel",
  }),
});
```
