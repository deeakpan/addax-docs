# Pair List

All markets currently available on Addax. Each pair has a `pairIndex` used on-chain and in the contracts, an asset class, and an oracle key used by the DIA price feed.

| Pair | Name | Class | pairIndex | Oracle key |
|---|---|---|---|---|
| BTC | Bitcoin | Crypto | 0 | `BTC/USD` |
| ETH | Ethereum | Crypto | 1 | `ETH/USD` |
| LTC | Litecoin | Crypto | 2 | `LTC/USD` |
| XAU | Gold | Commodity | 3 | `XAU/USD` |
| TSLA | Tesla | Equity | 4 | `TSLA` |
| SPCX | SpaceX | Equity | 5 | `SPCX` |
| SOL | Solana | Crypto | 6 | `SOL/USD` |

## Notes

- **pairIndex** is the canonical identifier passed to the trading and storage contracts. Use it when integrating (see [Trading Contracts](../developers/contracts.md)).
- **Oracle key** is the feed identifier on the DIA oracle (see [Price Oracle](../protocol/price-oracle.md)). Equities use bare keys (`TSLA`, `SPCX`); crypto and gold use `*/USD`.
- **Max leverage** and **spread** are configured per pair on-chain and may differ between crypto, commodities, and equities. See [Asset Classes](asset-classes.md).
- The list evolves as new markets are added; the app always reflects the live on-chain configuration.
