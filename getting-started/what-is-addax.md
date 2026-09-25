# What is Addax

{% tabs %}
{% tab title="Mainnet" %}
On mainnet, Addax is the interface for perpetual trading on Lighter. You keep a user-owned wallet, deposit USDC, and open positions against the Lighter order book.

## How a deposit works

1. You log in with MetaMask or a social account. Addax creates an embedded wallet for you, or restores the one already linked to that login.
2. USDC is bridged to that embedded wallet on Ethereum. The destination is always that address.
3. You approve a policy once. It allows Addax to pull the amount you chose and deposit it for you.
4. Addax sends that deposit and pays the Ethereum gas. Your balance is credited on Lighter to the same address.
5. You register an API key once. After that, each order is signed with the API key. Opening, closing, and canceling do not cost Ethereum gas.

A MetaMask login does not make MetaMask the trading key. MetaMask only identifies you. The embedded wallet is the address that receives funds and signs the deposit policy. A social login uses that same kind of embedded wallet.

## How a trade works

You choose a market, direction, size, and order type. The API key signs the order. When it fills, Addax can charge up to **0.1%** of the trade size in USDC. You agree to that maximum when you approve Addax. An order cannot charge more than the maximum you approved.

Leverage, margin, funding, and order behavior follow the Lighter market you trade.

Continue to [Network & Setup](network.md).
{% endtab %}

{% tab title="Testnet" %}
Addax is a decentralized leveraged trading platform on LitVM. It lets you take long or short positions on crypto, commodities, and equities with up to 100x leverage, using your own wallet and self-custodied collateral.

Addax does not use a traditional order book. Instead it settles trades synthetically against a **collateral vault** at an oracle-provided price. This design, pioneered by gTrade / Gains Network, which Addax is built on, gives every market deep and uniform liquidity, tight execution, and low fees, regardless of trade size relative to on-chain order-book depth.

## How it works at a glance

1. You deposit collateral (USDC, ADDX, or zkLTC) as margin and choose a market, direction, and leverage.
2. Addax opens a position priced from the **oracle mark price**, not from matching a counterparty.
3. Your profit or loss is paid from, or into, the collateral vault backing that market.
4. You can attach a **take-profit** and **stop-loss**, or place a **limit order** that only opens when price reaches your target.
5. Keeper bots monitor open positions and pending orders, and execute liquidations, limits, and TP/SL triggers automatically on-chain.

## Why synthetic + oracle pricing

| | Order-book DEX | Addax (synthetic) |
|---|---|---|
| Liquidity source | Resting limit orders | Collateral vault |
| Price | Best bid/ask | Oracle mark price |
| Slippage | Grows with size and thin books | None from book depth; only spread + price impact |
| Markets | Limited by market-maker presence | Any asset with a reliable price feed |
| Max leverage | Typically low | Up to 100x |

## The two sides of Addax

- **Traders** post margin and open leveraged positions. Their PnL flows to and from the vault.
- **Liquidity providers** deposit into **gToken vaults** (gUSDC, gzKLTC, gADDX). They act as the counterparty to all traders and earn from trading fees; over time the vault profits when traders net lose and draws down when traders net win.

## Trust model

- **Self-custody**: you always control your collateral through your own wallet.
- **On-chain settlement**: positions, orders, and PnL are enforced by smart contracts.
- **Oracle pricing**: marks come from **DIA** (push feeds at 1% deviation / 1h heartbeat on testnet), not from an internal book.
- **Indexed history**: protocol activity is indexed with **Goldsky** for reliable off-chain reads of trading events.
- **Permissionless keepers**: anyone can run a keeper to execute triggers and earn rewards.

Continue to [Network & Setup](network.md) to connect your wallet.
{% endtab %}
{% endtabs %}
