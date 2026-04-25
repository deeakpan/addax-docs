# Fetching Prices

## Off-chain quotes with QuoterV2

`AddaxV3QuoterV2` (`0x46D5676250100e6f9befF90455E032F85cC8775a`) lets you simulate a swap output without sending a transaction. Always use `staticCall` — the quoter reverts at the end of execution as a side effect of simulating the pool state.

### TypeScript

```typescript
import { ethers } from "ethers";

const provider = new ethers.JsonRpcProvider("https://liteforge.rpc.caldera.xyz/http");

const QUOTER_ABI = [
  "function quoteExactInputSingle(address tokenIn, address tokenOut, uint24 fee, uint256 amountIn, uint160 sqrtPriceLimitX96) returns (uint256 amountOut, uint160 sqrtPriceX96After, uint32 initializedTicksCrossed, uint256 gasEstimate)",
  "function quoteExactInput(bytes path, uint256 amountIn) returns (uint256 amountOut, uint160[] sqrtPriceX96AfterList, uint32[] initializedTicksCrossedList, uint256 gasEstimate)",
];

const quoter = new ethers.Contract(
  "0x46D5676250100e6f9befF90455E032F85cC8775a",
  QUOTER_ABI,
  provider
);

const WZKLTC = "0x6eF676c26E8C977554DA186eD0B215956E8F8753";
const USDC   = "0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50"; // aUSDC address on LitVM

// Quote: how much USDC do I get for 1 wzkLTC?
const [amountOut] = await quoter.quoteExactInputSingle.staticCall(
  WZKLTC,
  USDC,
  3000,                       // 0.3% fee tier
  ethers.parseEther("1"),     // 1 wzkLTC in
  0                           // no price limit
);

console.log(`1 wzkLTC = ${ethers.formatUnits(amountOut, 6)} USDC`);
```

### Python

```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://liteforge.rpc.caldera.xyz/http"))

QUOTER_ABI = [{"name":"quoteExactInputSingle","type":"function","stateMutability":"nonpayable","inputs":[{"name":"tokenIn","type":"address"},{"name":"tokenOut","type":"address"},{"name":"fee","type":"uint24"},{"name":"amountIn","type":"uint256"},{"name":"sqrtPriceLimitX96","type":"uint160"}],"outputs":[{"name":"amountOut","type":"uint256"},{"name":"sqrtPriceX96After","type":"uint160"},{"name":"initializedTicksCrossed","type":"uint32"},{"name":"gasEstimate","type":"uint256"}]}]

quoter = w3.eth.contract(
    address=Web3.to_checksum_address("0x46D5676250100e6f9befF90455E032F85cC8775a"),
    abi=QUOTER_ABI,
)

result = quoter.functions.quoteExactInputSingle(
    WZKLTC_ADDRESS,
    USDC_ADDRESS,
    3000,
    10**18,  # 1 wzkLTC
    0,
).call()

amount_out = result[0]
print(f"1 wzkLTC = {amount_out / 10**6} USDC")
```

## Reading pool price from slot0

For a real-time price without simulating a swap, read `slot0` directly from the pool:

```typescript
const FACTORY_ABI = ["function getPool(address, address, uint24) view returns (address)"];
const POOL_ABI = [
  "function slot0() view returns (uint160 sqrtPriceX96, int24 tick, uint16 observationIndex, uint16 observationCardinality, uint16 observationCardinalityNext, uint8 feeProtocol, bool unlocked)",
  "function liquidity() view returns (uint128)",
];

const factory = new ethers.Contract("0x8b84D582622DfF7bC3582365941791B68Ae70f4d", FACTORY_ABI, provider);
const poolAddress = await factory.getPool(WZKLTC, USDC, 3000);
const pool = new ethers.Contract(poolAddress, POOL_ABI, provider);

const [sqrtPriceX96, tick] = await pool.slot0();

// Convert sqrtPriceX96 to human-readable price
// price = (sqrtPriceX96 / 2^96)^2 * 10^(decimals0 - decimals1)
const Q96 = 2n ** 96n;
const price = (sqrtPriceX96 * sqrtPriceX96 * BigInt(10 ** 6)) / (Q96 * Q96 * BigInt(10 ** 18));
console.log(`Pool price: ${price} USDC per wzkLTC`);
```

## Multi-hop quotes

For a path through multiple pools (e.g. tokenA → wzkLTC → USDC):

```typescript
import { encodePath } from "@uniswap/v3-sdk"; // or encode manually

const path = encodePath(
  [TOKEN_A, WZKLTC, USDC],
  [3000, 3000]
);

const [amountOut] = await quoter.quoteExactInput.staticCall(path, amountIn);
```
