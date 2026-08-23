# Asset Classes

Addax lists markets across several asset classes. All are traded the same way, synthetically, against the collateral vault, at the oracle mark price, but they differ in trading hours behavior, volatility, and typical max leverage.

## Crypto

Digital assets such as **BTC, ETH, LTC, and SOL**. Crypto markets are priced 24/7 and generally support the **highest leverage** on the platform. They are the most liquid and actively traded class on Addax.

## Commodities

Real-world commodities such as **gold (XAU)**. Priced from off-chain oracle feeds, commodities offer exposure to macro/hard-asset moves. Leverage caps are typically lower than crypto, reflecting different volatility and market-hours dynamics.

## Equities

Individual stocks such as **Tesla (TSLA)** and pre-IPO/private names like **SpaceX (SPCX)**. Equity markets let you take leveraged directional views on companies without holding the underlying shares. Because equities have defined trading sessions and different risk profiles, they usually carry the most conservative leverage caps.

## How class affects trading

| | Crypto | Commodities | Equities |
|---|---|---|---|
| Pricing hours | 24/7 | Oracle-dependent | Oracle-dependent |
| Typical max leverage | Highest | Medium | Lowest |
| Volatility | High | Medium | Varies |

Max leverage and spread are set **per pair** on-chain, always check the live values in the app or the [Pair List](pair-list.md). For how prices are sourced across classes, see [Price Oracle](../protocol/price-oracle.md).
