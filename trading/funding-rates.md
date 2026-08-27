# Funding Rates

Funding balances long and short open interest on each market. It is a transfer **between traders** — not a protocol fee — and it is the one cost on Addax that can work in your favour.

## How funding works

While a position is open, funding accrues continuously on **position size** (collateral × leverage).

- If the market is **net long**, longs pay shorts.
- If the market is **net short**, shorts pay longs.
- If long and short open interest are roughly equal, funding is **near zero**.

Opening on the lighter side of an imbalanced market means you **earn** funding for as long as that imbalance lasts. Opening on the crowded side means you **pay**.

Funding is settled into the position (margin / PnL). There is no separate hourly wallet transfer. Paying funding slowly reduces margin and can move liquidation closer; earning funding does the opposite.

## Example rates

Base rates are set per market from volatility and updated periodically. The figures below are the **maximum rate on net open interest** when one side is empty. Your effective rate scales with how skewed the market is.

<p>
  <img src="assets/btc.png" alt="BTC" width="28" height="28" />
  <strong> BTC/USD</strong><br/>
  ~0.0013% / hour &nbsp;·&nbsp; ~11.2% / year (full skew)
</p>

<p>
  <img src="assets/sol.png" alt="SOL" width="28" height="28" />
  <strong> SOL/USD</strong><br/>
  ~0.0018% / hour &nbsp;·&nbsp; ~15.9% / year (full skew)
</p>

<p>
  <img src="assets/hype.jpg" alt="HYPE" width="28" height="28" />
  <strong> HYPE/USD</strong><br/>
  ~0.0030% / hour &nbsp;·&nbsp; ~26.1% / year (full skew)
</p>

**Example.** Suppose BTC/USD funding is ~11%/year at full skew, with **\$1m** long and **\$500k** short open interest:

- Net exposure = \$500k
- Total funding transferred ≈ \$500k × 11% ≈ **\$55k / year**
- Longs pay ~**5.5%/year** on their OI; shorts earn ~**11%/year** on theirs

If the same market were perfectly balanced, both sides would pay ~**0**.

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
