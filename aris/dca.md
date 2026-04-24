# DCA Orders

Dollar-cost averaging (DCA) on Addax splits a total buy into equal slices executed at regular intervals. Each slice is a separate signed order. Fillers execute each slice within its allowed time window.

**Reactor:** `ArisDcaOrderReactor` — `0x99D007fD09211B437Ecc415bB8D08Cc68b877672`

## How it works

From `DcaOrderLib.sol`, the DCA order encodes the full schedule:

```solidity
struct DcaOrder {
    OrderInfo info;
    address inputToken;       // token being sold each slice
    uint256 inputAmount;      // amount sold per slice
    OutputToken[] outputs;    // minimum received per slice
    uint256 startTime;        // unix timestamp when the first slice opens
    uint256 intervalSeconds;  // seconds between slice windows
    uint256 sliceIndex;       // which slice this signed order represents (0-based)
    uint256 totalSlices;      // total number of slices in the strategy
    uint256 fillWindowSeconds; // how long a filler has to fill each slice (0 = full interval)
}
```

The reactor validates timing per slice:

```
sliceStart = startTime + (sliceIndex × intervalSeconds)
sliceEnd   = sliceStart + fillWindowSeconds
```

A filler can only call `execute()` with slice N when `block.timestamp` is between `sliceStart` and `sliceEnd`. Early or late fills revert.

## Concrete example

**Goal:** Buy wzkLTC with 700 USDC over 7 days — 100 USDC per day.

The user signs 7 separate `DcaOrder` messages, one per slice:

| Slice | sliceIndex | Execution window |
|---|---|---|
| 1 | 0 | Day 1 00:00 → Day 1 23:59 |
| 2 | 1 | Day 2 00:00 → Day 2 23:59 |
| 3 | 2 | Day 3 00:00 → Day 3 23:59 |
| … | … | … |
| 7 | 6 | Day 7 00:00 → Day 7 23:59 |

Each slice order:
- `inputToken`: USDC
- `inputAmount`: `100_000_000` (100 USDC, 6 decimals)
- `outputs[0].token`: wzkLTC
- `outputs[0].amount`: minimum wzkLTC acceptable for that 100 USDC
- `startTime`: start of day 1 (unix)
- `intervalSeconds`: `86400` (1 day)
- `fillWindowSeconds`: `82800` (23 hours — gives fillers most of the day)
- `totalSlices`: `7`

All 7 signed orders are submitted to the API at once. Fillers pick them up and execute each one on the correct day. The user only needs to sign once and have 700 USDC approved on Permit2.

## Submitting DCA orders (TypeScript)

```typescript
const slices = 7;
const interval = 86400; // 1 day in seconds
const startTime = Math.floor(Date.now() / 1000) + 60; // starts in 1 minute

for (let i = 0; i < slices; i++) {
  const order = {
    info: {
      reactor: DCA_REACTOR,
      swapper: userAddress,
      nonce: baseNonce + i,
      deadline: startTime + interval * slices,
      additionalValidationContract: ethers.ZeroAddress,
      additionalValidationData: "0x",
    },
    inputToken: USDC_ADDRESS,
    inputAmount: ethers.parseUnits("100", 6),
    outputs: [{ token: WZKLTC, amount: ethers.parseEther("0.05"), recipient: userAddress }],
    startTime,
    intervalSeconds: interval,
    sliceIndex: i,
    totalSlices: slices,
    fillWindowSeconds: 82800,
  };

  const encodedOrder = encodeOrder(order); // ABI encode the DcaOrder struct
  const signature = await signer.signTypedData(domain, dcaTypes, order);

  await fetch("https://addax.finance/api/dca", {
    method: "POST",
    body: JSON.stringify({
      orderHash: ethers.keccak256(encodedOrder),
      encodedOrder,
      signature,
      orderType: "DCA",
      offerer: userAddress,
      expiresAt: new Date((startTime + interval * slices) * 1000).toISOString(),
      chainId: 4441,
    }),
  });
}
```
