# Funding Rates

Funding balances long and short open interest on each market. It is a transfer **between traders** — not a protocol fee — and it is the one cost on Addax that can work in your favour.

## How funding works

While a position is open, funding accrues continuously on **position size** (collateral × leverage).

- If the market is **net long**, longs pay shorts.
- If the market is **net short**, shorts pay longs.
- If long and short open interest are roughly equal, funding is **near zero**.

Opening on the lighter side of an imbalanced market means you **earn** funding for as long as that imbalance lasts. Opening on the crowded side means you **pay**.

Funding is settled into the position (margin / PnL). There is no separate hourly wallet transfer. Paying funding slowly reduces margin and can move liquidation closer; earning funding does the opposite.

## Rates

Base rates are set per market from volatility and updated periodically. Values below are the **maximum rate on net open interest** when one side is empty. Your effective rate scales with how skewed the market is.

| | Market | Hourly | Annual (full skew) |
| :---: | --- | ---: | ---: |
| <img src="assets/btc.png" alt="BTC" width="24" /> | BTC/USD | ~0.0013% | ~11.2% |
| <img src="assets/sol.png" alt="SOL" width="24" /> | SOL/USD | ~0.0018% | ~15.9% |
| <img src="assets/hype.jpg" alt="HYPE" width="24" /> | HYPE/USD | ~0.0030% | ~26.1% |

With **\$1m** long and **\$500k** short on BTC/USD at ~11%/year full skew: net exposure is \$500k, so ~\$55k/year moves from longs to shorts (~5.5%/year cost for longs, ~11%/year for shorts). A balanced market pays ~0.

Higher-volatility markets (e.g. HYPE) carry a higher base rate than majors like BTC; calmer markets (e.g. gold) sit lower. Live skew still dominates what you actually pay or earn.

## Reading funding in the app

| Display | Meaning |
|---|---|
| Longs pay / shorts earn | Market is net long |
| Shorts pay / longs earn | Market is net short |
| Near zero | Long and short OI are balanced |

## Related

- [Fees & Spread](fees-and-spread.md)
- [Leverage & Liquidation](leverage-and-liquidation.md)
- [Protocol Fees](../protocol/fees.md)
