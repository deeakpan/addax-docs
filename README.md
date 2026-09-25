# Addax Docs

{% tabs %}

{% tab title="Mainnet" %}

Addax is a perpetual order book. You post a bid or an ask, or you take the price already resting on the book. A market order fills immediately, down to the worst price you allow. A limit order rests until it trades, expires, or you cancel it.

Stops and take profits sit off the book until their trigger price is hit, then they send a market or limit order. A TWAP splits a larger order into smaller fills over time.

Funding is paid between traders once an hour. When the rate is positive, longs pay shorts. When it is negative, shorts pay longs. Addax charges up to 0.1% of the trade size when an order fills.

Leverage depends on the market. Major crypto markets go up to 50x.

Addax is built on [Lighter](https://lighter.xyz/).

{% endtab %}

{% tab title="Testnet" %}

Addax is a decentralized leveraged trading platform on **LitVM**. Trade crypto, commodities, and equities with up to 100x leverage, directly from your wallet. No sign-up, no custody, no order book.

Addax uses a synthetic, oracle-priced trading model: instead of matching buyers and sellers, trades settle against a collateral vault at the oracle mark price. This lets Addax offer deep, uniform liquidity across every market and low, predictable fees.

Marks are supplied by **DIA**. On-chain trading activity is indexed with **Goldsky**. See [Architecture](protocol/overview.md) and [Price Oracle](protocol/price-oracle.md) for how the stack is put together.

## What you can do

| | |
|---|---|
| **Trade** | Long or short crypto, gold, and stocks with 1x–100x leverage |
| **Order types** | Market, limit, stop-loss and take-profit |
| **Provide liquidity** | Deposit into gToken vaults (gUSDC, gzKLTC, gADDX) and earn from trading activity |
| **Multiple collaterals** | Open trades with USDC, ADDX, or zkLTC |

## Where to start

- New here? Read [What is Addax](getting-started/what-is-addax.md).
- Ready to trade? See [Setting up to trade](getting-started/setting-up-to-trade.md) then [Opening & closing trades](trading/opening-closing-trades.md).
- Want to earn? Go to [gToken Vaults](vaults/overview.md).
- Building an integration or bot? Start with [Developers](developers/README.md).

> Addax is currently deployed on the LitVM testnet. All tokens are testnet assets with no monetary value.

{% endtab %}

{% endtabs %}
