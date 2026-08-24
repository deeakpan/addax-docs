# Direct Contract Integration

Use this page when you need full on-chain control: custom keepers, direct RPC reads, or flows not yet covered by the [Trading API](./trading-api.md). For most apps, prefer the Trading API — it resolves addresses, validates parameters, and returns ready-to-sign calldata.

Contract addresses: [Contracts & Addresses](../protocol/contracts.md). Examples target the **gUSDC stack** on LitVM (chain ID `4441`).

## Contract roles

| Contract | Address (gUSDC) | Use it to |
|---|---|---|
| **Trading** | `0x468A8eFB014bc7784C3BD1F6F3a7cf7feB07B1e8` | Open, close, TP/SL, limits |
| **Storage** | `0xEEC2067f8a310B2b09f9b97eC4c5247250D2c712` | Read open trades and limits |
| **Pair Infos** | `0xa574dAE7EbF8cA56a9AC80932bFf9862C6D62FFC` | Spread, fees, leverage caps |
| **Price Aggregator** | `0xA184242a075bEA7012Ce83BD86f3E56a9bc33A73` | Oracle fulfillment |
| **Vault (gUSDC)** | `0xbA68d137F6AaD10a7490DDb94bbd718f59b6A1C6` | LP deposits / withdrawals |
| **USDC collateral** | `0xA6b7A782Fc4349914dADde5b8A8A8B1daDFBF6DB` | Margin token (6 decimals) |

## Setup (viem)

```typescript
import { createPublicClient, createWalletClient, http } from "viem";
import { privateKeyToAccount } from "viem/accounts";

const RPC = "https://liteforge.rpc.caldera.xyz/http";
const CHAIN_ID = 4441;

const publicClient = createPublicClient({ transport: http(RPC) });
const account = privateKeyToAccount(process.env.PRIVATE_KEY as `0x${string}`);
const walletClient = createWalletClient({ account, transport: http(RPC) });

const TRADING = "0x468A8eFB014bc7784C3BD1F6F3a7cf7feB07B1e8";
const USDC = "0xA6b7A782Fc4349914dADde5b8A8A8B1daDFBF6DB";
```

Import ABIs from your contract artifacts or the gTrade interface. The Addax app ships a trimmed trading ABI for `openTrade`, `closeTradeMarket`, `updateTp`, `updateSl`, and `cancelOpenLimitOrder`.

## Trade struct

gTrade v6 `openTrade` accepts a trade tuple plus order metadata:

| Field | Type | Notes |
|---|---|---|
| `trader` | `address` | Position owner |
| `pairIndex` | `uint256` | Market index ([Pair List](../trading/pair-list.md)) |
| `index` | `uint256` | Trade slot (usually `0`) |
| `initialPosToken` | `uint256` | `0` for USDC margin |
| `positionSizeDai` | `uint256` | Collateral in 1e18 DAI units (`100 USDC` → `100e18`) |
| `openPrice` | `uint256` | 1e10-scaled price |
| `buy` | `bool` | `true` = long |
| `leverage` | `uint256` | Integer leverage |
| `tp` / `sl` | `uint256` | 1e10-scaled TP/SL (`0` = unset) |

Additional args: `orderType` (`0` market, `1` limit), `spreadReductionId`, `slippageP`, `referral`.

## Open a market long

```typescript
import { parseUnits } from "viem";

// 100 USDC margin @ 10× on BTC (pairIndex 0)
const marginUsdc = parseUnits("100", 6);
const positionSizeDai = marginUsdc * 1_000_000_000_000n; // USDC → gTrade dai scale
const openPrice = 95_000_0000000000n; // $95,000 at 1e10 precision
const slippageP = 5_000_000_000n; // 5% (5 × 1e10 / 100)

const trade = {
  trader: account.address,
  pairIndex: 0n,
  index: 0n,
  initialPosToken: 0n,
  positionSizeDai,
  openPrice,
  buy: true,
  leverage: 10n,
  tp: 0n,
  sl: 0n,
} as const;

const { request } = await publicClient.simulateContract({
  account: account.address,
  address: TRADING,
  abi: tradingAbi,
  functionName: "openTrade",
  args: [trade, 0, 0n, slippageP, "0x0000000000000000000000000000000000000000"],
});

const hash = await walletClient.writeContract(request);
```

Market orders settle in the same transaction once the oracle mark is available.

## Close, update, cancel

```typescript
// Close full position
await walletClient.writeContract({
  address: TRADING,
  abi: tradingAbi,
  functionName: "closeTradeMarket",
  args: [0n, 0n], // pairIndex, tradeIndex
});

// Update take-profit to $75,000
await walletClient.writeContract({
  address: TRADING,
  abi: tradingAbi,
  functionName: "updateTp",
  args: [0n, 0n, 75_000_0000000000n],
});

// Cancel pending limit
await walletClient.writeContract({
  address: TRADING,
  abi: tradingAbi,
  functionName: "cancelOpenLimitOrder",
  args: [0n, 0n],
});
```

## Read open trades (Storage)

```typescript
const STORAGE = "0xEEC2067f8a310B2b09f9b97eC4c5247250D2c712";

const count = await publicClient.readContract({
  address: STORAGE,
  abi: storageAbi,
  functionName: "openTradesCount",
  args: [traderAddress, 0n],
});

const hasLimit = await publicClient.readContract({
  address: STORAGE,
  abi: storageAbi,
  functionName: "hasOpenLimitOrder",
  args: [traderAddress, 0n, 0n],
});
```

## Prefer the Trading API?

If you do not need raw contract access, use [Trading API](./trading-api.md) `POST /orders/prepare` — it returns the same calldata with validation, gas estimates, and contract addresses bundled in the response.

## Related

- [Trading API](./trading-api.md) — prepared transactions (recommended)
- [Public REST API](./public-api.md) — read-only HTTP endpoints
- [Keepers](../protocol/keepers.md) — trigger execution flow
- [addax-protocol/bots](https://github.com/addax-protocol/bots) — reference keeper implementations
