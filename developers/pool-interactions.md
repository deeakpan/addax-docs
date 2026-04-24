# Pool Interactions

## Creating a pool

```typescript
const FACTORY_ABI = [
  "function createPool(address tokenA, address tokenB, uint24 fee) returns (address pool)",
  "function getPool(address, address, uint24) view returns (address)",
];

const factory = new ethers.Contract(
  "0x8E15B5482B201209300805A8eCD09cD1dA5800cc",
  FACTORY_ABI,
  signer
);

const tx = await factory.createPool(WZKLTC, USDC, 3000);
await tx.wait();

const poolAddress = await factory.getPool(WZKLTC, USDC, 3000);
```

## Initializing pool price

A new pool has no price. The first LP must initialize it before adding liquidity:

```typescript
const POOL_ABI = ["function initialize(uint160 sqrtPriceX96)"];
const pool = new ethers.Contract(poolAddress, POOL_ABI, signer);

// Set initial price: 1 wzkLTC = 100 USDC
// sqrtPriceX96 = sqrt(price * 10^(decimals1 - decimals0)) * 2^96
// For wzkLTC (18 dec) / USDC (6 dec): price = 100, decimals adjustment = 10^(6-18) = 10^-12
// sqrtPriceX96 = sqrt(100 * 10^-12) * 2^96 ≈ 7.9E27
const sqrtPriceX96 = BigInt("7922816251426433759354395033600"); // ~100 USDC per wzkLTC
await pool.initialize(sqrtPriceX96);
```

## Minting a position

```typescript
const NPM_ABI = [
  "function mint((address token0, address token1, uint24 fee, int24 tickLower, int24 tickUpper, uint256 amount0Desired, uint256 amount1Desired, uint256 amount0Min, uint256 amount1Min, address recipient, uint256 deadline)) returns (uint256 tokenId, uint128 liquidity, uint256 amount0, uint256 amount1)",
];

// Approve NPM to spend your tokens first
await wzkLTC.approve("0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d", ethers.MaxUint256);
await usdc.approve("0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d", ethers.MaxUint256);

const npm = new ethers.Contract("0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d", NPM_ABI, signer);

const { tokenId } = await npm.mint({
  token0: WZKLTC,
  token1: USDC,
  fee: 3000,
  tickLower: -887220,   // wide range (full range for 0.3% pool)
  tickUpper: 887220,
  amount0Desired: ethers.parseEther("10"),       // 10 wzkLTC
  amount1Desired: ethers.parseUnits("1000", 6),  // 1000 USDC
  amount0Min: 0,
  amount1Min: 0,
  recipient: userAddress,
  deadline: Math.floor(Date.now() / 1000) + 600,
});
```

## Collecting fees

```typescript
const COLLECT_ABI = [
  "function collect((uint256 tokenId, address recipient, uint128 amount0Max, uint128 amount1Max)) returns (uint256 amount0, uint256 amount1)",
];

const npm = new ethers.Contract("0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d", COLLECT_ABI, signer);

await npm.collect({
  tokenId: myTokenId,
  recipient: userAddress,
  amount0Max: (2n ** 128n) - 1n,
  amount1Max: (2n ** 128n) - 1n,
});
```

## Executing a swap

```typescript
const ROUTER_ABI = [
  "function exactInputSingle((address tokenIn, address tokenOut, uint24 fee, address recipient, uint256 deadline, uint256 amountIn, uint256 amountOutMinimum, uint160 sqrtPriceLimitX96)) returns (uint256 amountOut)",
];

await usdc.approve("0xD3B3b516d159AC5D0ddB579AA6feD92598202B55", amount);

const router = new ethers.Contract("0xD3B3b516d159AC5D0ddB579AA6feD92598202B55", ROUTER_ABI, signer);

const amountOut = await router.exactInputSingle({
  tokenIn: USDC,
  tokenOut: WZKLTC,
  fee: 3000,
  recipient: userAddress,
  deadline: Math.floor(Date.now() / 1000) + 300,
  amountIn: ethers.parseUnits("100", 6),
  amountOutMinimum: 0,
  sqrtPriceLimitX96: 0,
});
```
