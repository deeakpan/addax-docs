# Fetching Prices

Addax marks come from **DIA**. On LitVM testnet, feeds are **push**-updated with a **0.1%** deviation threshold; mainnet is planned around DIA **pull** reports. The **Price Aggregator** consumes those feeds for each market. This page shows how to read marks for integrations. Full oracle design: [Price Oracle](../protocol/price-oracle.md).

## Market -> oracle key

Each pair maps to an oracle key and a `pairIndex`:

| Pair | pairIndex | Oracle key |
|---|---|---|
| BTC | 0 | `BTC/USD` |
| ETH | 1 | `ETH/USD` |
| LTC | 2 | `LTC/USD` |
| XAU | 3 | `XAU/USD` |
| TSLA | 4 | `TSLA/USD` |
| SPCX | 5 | `SPCX/USD` |
| SOL | 6 | `SOL/USD` |
| HYPE | 7 | `HYPE/USD` |

Full list: [Pair List](../trading/pair-list.md).

## Reading from the DIA oracle

The DIA oracle exposes values by key. Read the latest price for a market:

```typescript
import { createPublicClient, http } from "viem";

const client = createPublicClient({ transport: http("https://liteforge.rpc.caldera.xyz/http") });

const DIA_ABI = [
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
 address: "0xFf856a958eFA7965A4dFC2BFb09dDbc9EABe9aAb",
 abi: DIA_ABI,
 functionName: "getValue",
 args: ["BTC/USD"],
});
```

> Confirm the oracle interface and value scaling against the deployed contract, DIA-style feeds typically report 8-decimal values with a publish timestamp.

## Public REST API

Base URL:

```text
https://addax.finance
```

Convenience endpoints for marks, positions, and related market data:

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/perp/market-stats` | Per-market mark price, change, and stats |
| `GET` | `/api/perp/vault-mark` | On-chain vault / DIA mark (`?symbol=BTC` or `?diaKey=BTC/USD`) |
| `GET` | `/api/perp/positions` | Open positions for an account (`?account=0x…`) |
| `GET` | `/api/perp/open-limits` | Pending open limit orders (`?account=0x…`) |
| `GET` | `/api/perp/trades` | Trade history (`?account=0x…`, optional `symbol`, `limit`) |
| `GET` | `/api/perp/candles` | OHLCV candles (`?asset=bitcoin&interval=15m`) |
| `GET` | `/api/perp/prices` | Reference spot quotes for header assets |

Example:

```bash
curl "https://addax.finance/api/perp/market-stats"
curl "https://addax.finance/api/perp/positions?account=0xYourAddress"
curl "https://addax.finance/api/perp/vault-mark?symbol=BTC"
```

These are the simplest way to display prices and positions without wiring up oracle reads yourself.

## Mark vs execution price

The oracle returns the **mark price**. Execution applies **spread** (and price impact for large size) on top, longs open slightly above mark, shorts slightly below. Pull spread/fee parameters from the **Pair Infos** contract. See [Fees & Spread](../trading/fees-and-spread.md).
