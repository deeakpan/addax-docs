# Building Bots

## Price monitoring bot

Poll the pool's `slot0` every block and alert when price moves beyond a threshold.

```typescript
import { ethers } from "ethers";

const provider = new ethers.JsonRpcProvider("https://liteforge.rpc.caldera.xyz/http");

const POOL_ABI = [
  "function slot0() view returns (uint160 sqrtPriceX96, int24 tick, uint16, uint16, uint16, uint8, bool)",
  "event Swap(address indexed sender, address indexed recipient, int256 amount0, int256 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick)",
];

const pool = new ethers.Contract(POOL_ADDRESS, POOL_ABI, provider);

// Listen to Swap events in real-time
pool.on("Swap", (sender, recipient, amount0, amount1, sqrtPriceX96, liquidity, tick) => {
  const price = tickToPrice(Number(tick), 18, 6);
  console.log(`Swap — new price: ${price} USDC/wzkLTC at tick ${tick}`);
});

function tickToPrice(tick: number, decimals0: number, decimals1: number): number {
  return Math.pow(1.0001, tick) * Math.pow(10, decimals0 - decimals1);
}
```

## LP rebalancing bot

Check if your position is out of range and re-mint at current price.

```typescript
async function checkAndRebalance(tokenId: bigint) {
  const [sqrtPriceX96, currentTick] = await pool.slot0();
  const position = await npm.positions(tokenId);

  const inRange = currentTick >= position.tickLower && currentTick < position.tickUpper;

  if (!inRange) {
    console.log("Position out of range — rebalancing...");

    // 1. Remove all liquidity
    await npm.decreaseLiquidity({
      tokenId,
      liquidity: position.liquidity,
      amount0Min: 0,
      amount1Min: 0,
      deadline: Math.floor(Date.now() / 1000) + 300,
    });

    // 2. Collect tokens + fees
    await npm.collect({
      tokenId,
      recipient: walletAddress,
      amount0Max: (2n ** 128n) - 1n,
      amount1Max: (2n ** 128n) - 1n,
    });

    // 3. Re-mint at new range centred on currentTick
    const SPACING = 60; // for 0.3% pool
    const width = 600;
    const tickLower = Math.floor((Number(currentTick) - width) / SPACING) * SPACING;
    const tickUpper = Math.ceil((Number(currentTick) + width) / SPACING) * SPACING;

    await npm.mint({
      token0: WZKLTC, token1: USDC, fee: 3000,
      tickLower, tickUpper,
      amount0Desired: await wzkLTC.balanceOf(walletAddress),
      amount1Desired: await usdc.balanceOf(walletAddress),
      amount0Min: 0, amount1Min: 0,
      recipient: walletAddress,
      deadline: Math.floor(Date.now() / 1000) + 300,
    });
  }
}
```

## Arbitrage bot

Watch for price divergence between Addax and external venues.

```typescript
async function checkArbitrage() {
  // Get Addax price
  const [addaxOut] = await quoter.quoteExactInputSingle.staticCall(
    WZKLTC, USDC, 3000, ethers.parseEther("1"), 0
  );

  // Get external venue price (implement for each venue)
  const externalPrice = await getExternalPrice();

  const addaxPrice = Number(ethers.formatUnits(addaxOut, 6));
  const spread = Math.abs(addaxPrice - externalPrice) / externalPrice;

  if (spread > 0.005) { // 0.5% threshold
    console.log(`Arb opportunity: Addax ${addaxPrice} vs external ${externalPrice}`);
    // Execute the arb via aggregator or direct router
  }
}
```

## Fee collection bot

Periodically collect fees from all your LP positions.

```typescript
const ERC721_ABI = ["function tokenOfOwnerByIndex(address, uint256) view returns (uint256)", "function balanceOf(address) view returns (uint256)"];
const npm_full = new ethers.Contract("0x049293EcBc8771488aD16EeDE035e14342D60D9F", [...ERC721_ABI, ...COLLECT_ABI], signer);

async function collectAllFees() {
  const balance = await npm_full.balanceOf(walletAddress);

  for (let i = 0; i < balance; i++) {
    const tokenId = await npm_full.tokenOfOwnerByIndex(walletAddress, i);

    const tx = await npm_full.collect({
      tokenId,
      recipient: walletAddress,
      amount0Max: (2n ** 128n) - 1n,
      amount1Max: (2n ** 128n) - 1n,
    });
    await tx.wait();
    console.log(`Collected fees for position ${tokenId}`);
  }
}

// Run daily
setInterval(collectAllFees, 86_400_000);
```
