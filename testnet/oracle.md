# DIA prices

Testnet marks come from DIA. Opens, closes, and triggers use the price stored on LitVM. A feed updates when the price moves by 1%, or at least every hour.

| Parameter | Value |
|---|---|
| Oracle | `0xEd7f45c29FE6676e1eB7096aD5D6966abd62Bd1a` |
| Deviation | 1% |
| Heartbeat | 1 hour |

| Ticker | Key |
|---|---|
| BTC | `BTC/USD` |
| LTC | `LTC/USD` |
| ETH | `ETH/USD` |
| SOL | `SOL/USD` |
| NVDA | `NVDA` |
| TSLA | `TSLA` |
| AAPL | `AAPL` |
| MSFT | `MSFT` |
| SPCX | `SPCX` |
| XAU | `XAU/USD` |
| CPER | `CPER` |
| WTI | `WTI/USD` |
| NG | `NG` |
| JPY | `USD/JPY` |

Mainnet does not use these feeds. Mainnet prices are the order book mark and index. Addax is built on [Lighter](https://lighter.xyz/).
