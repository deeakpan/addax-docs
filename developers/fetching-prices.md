# Fetching Prices

Addax marks come from **DIA**. On LitVM testnet, feeds are **push**-updated with a **1%** deviation threshold and a **1h** heartbeat; mainnet is planned around DIA **pull** reports.

## Recommended: Public REST API

For apps and dashboards, use the [Public REST API](./public-api.md):

```bash
curl "https://addax.finance/api/perp/vault-mark?symbol=BTC"
curl "https://addax.finance/api/perp/market-stats"
```

These endpoints are rate-limited (60 req/min per IP). For production volume, use the [Trading API](./trading-api.md).

## Market → oracle key

| Pair | pairIndex | Oracle key |
|---|---|---|
| BTC | 0 | `BTC/USD` |
| ETH | 1 | `ETH/USD` |
| LTC | 2 | `LTC/USD` |
| XAU | 3 | `XAU/USD` |
| TSLA | 4 | `TSLA` |
| SPCX | 5 | `SPCX` |
| SOL | 6 | `SOL/USD` |

Full DIA feed inventory: [Price Oracle](../protocol/price-oracle.md). Pair metadata: [Pair List](../trading/pair-list.md).

## Direct on-chain read (DIA)

```typescript
import { createPublicClient, http } from "viem";

const client = createPublicClient({
  transport: http("https://liteforge.rpc.caldera.xyz/http"),
});

const DIA_ORACLE = "0xEd7f45c29FE6676e1eB7096aD5D6966abd62Bd1a";

const diaAbi = [
  {
    type: "function",
    name: "getValue",
    stateMutability: "view",
    inputs: [{ name: "key", type: "string" }],
    outputs: [
      { name: "value", type: "uint128" },
      { name: "timestamp", type: "uint128" },
    ],
  },
] as const;

const [value, timestamp] = await client.readContract({
  address: DIA_ORACLE,
  abi: diaAbi,
  functionName: "getValue",
  args: ["BTC/USD"],
});
```

Confirm decimals and staleness rules against the live contract before hardcoding display math. Full oracle design: [Price Oracle](../protocol/price-oracle.md).

## Mark vs execution price

The oracle returns the **mark price**. Execution applies **spread** (and price impact for large size) on top — longs open slightly above mark, shorts slightly below. Pull spread/fee parameters from **Pair Infos**. See [Fees & Spread](../trading/fees-and-spread.md).

## Related

- [Public REST API](./public-api.md)
- [Trading API](./trading-api.md)
- [Direct Contract Integration](./contracts.md)
