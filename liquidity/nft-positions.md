# NFT Positions

Every LP position on Addax is minted as an ERC-721 NFT by the `AddaxV3NonfungiblePositionManager` (`0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d`). The NFT encodes all parameters of your position and can be transferred, held in a wallet, or managed through the Addax interface.

## What the NFT stores

- Pool address (derived from token0, token1, fee)
- `tickLower` and `tickUpper` — your price range bounds
- `liquidity` — your share of the pool
- `tokensOwed0` / `tokensOwed1` — accumulated uncollected fees

## Position lifecycle

**Mint** — Deposit both tokens to open a new position within a price range.

**Increase liquidity** — Add more of both tokens to an existing position, increasing your share at the same range.

**Decrease liquidity** — Remove a portion of your liquidity. Tokens are returned to your wallet. Fees are not automatically collected at this step.

**Collect** — Withdraw accumulated swap fees from the position. Can be called without changing liquidity.

**Burn** — After fully removing liquidity and collecting fees, burn the NFT to clean up. The token ID becomes invalid.

## Key contract calls

```typescript
const NPM_ADDRESS = "0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d";

// Mint a new position
await npm.mint({
  token0: WZKLTC,
  token1: USDC,
  fee: 3000,
  tickLower: -887220,
  tickUpper: 887220,
  amount0Desired: ethers.parseEther("1"),
  amount1Desired: ethers.parseUnits("90", 6),
  amount0Min: 0,
  amount1Min: 0,
  recipient: userAddress,
  deadline: Math.floor(Date.now() / 1000) + 600,
});

// Collect fees
await npm.collect({
  tokenId: myTokenId,
  recipient: userAddress,
  amount0Max: ethers.MaxUint128,
  amount1Max: ethers.MaxUint128,
});
```
