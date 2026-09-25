# Setting Up to Trade

{% tabs %}
{% tab title="Mainnet" %}
## 1. Log in

Open Addax and log in with MetaMask or a social account. **[Para](https://www.getpara.com/)** opens your embedded wallet. If you have logged in this way before, you get the same wallet back.

## 2. Deposit USDC

Send USDC to that wallet on Ethereum. If the funds start on another supported chain, the bridge delivers them to the same address. You approve the deposit policy once. Addax submits the deposit and pays the Ethereum gas.

## 3. Register an API key

You sign once to attach an API key to your account. That key signs later orders. You do not pay Ethereum gas to open, close, or cancel.

## 4. Place a trade

1. Pick a market.
2. Choose long or short.
3. Set size and order type.
4. Confirm. The API key signs the order.

When the order fills, Addax can charge up to 0.1% of the trade size in USDC.

> Mainnet deposits and trading described here are the planned flow. They are not live yet.
{% endtab %}

{% tab title="Testnet" %}
Follow these steps to place your first trade on Addax.

## 1. Connect your wallet

Open the Addax app and connect an EVM wallet (MetaMask, Rabby, WalletConnect, etc.). Approve the prompt to add or switch to the **LitVM** network (chain ID `4441`).

## 2. Get gas

Every transaction costs a small amount of native **zkLTC** for gas. Claim testnet zkLTC from the LitVM faucet, see [Get Testnet zkLTC](faucet.md).

## 3. Get collateral

You can open trades with any of the supported collaterals:

| Collateral | Vault | Notes |
|---|---|---|
| **USDC** | gUSDC | Stablecoin margin; has a testnet faucet |
| **ADDX** | gADDX | Native protocol token |
| **zkLTC / WzkLTC** | gzKLTC | Native gas token; wrapped to WzkLTC for margin |

See [Collateral & Tokens](tokens.md) for addresses and how to obtain each.

## 4. Approve your collateral

The first time you trade with a given collateral, you'll sign a one-time ERC-20 **approval** so the trading contract can pull your margin. This is a per-token, per-stack approval.

## 5. Place a trade

1. Pick a market (e.g. BTC, ETH, LTC, XAU, TSLA).
2. Choose **Long** or **Short**.
3. Set your **collateral amount** and **leverage** (1x–100x).
4. Optionally set a **limit price**, **take-profit**, and **stop-loss**.
5. Confirm the transaction.

For a full walkthrough, continue to [Opening & Closing Trades](../trading/opening-closing-trades.md).

> **Testnet reminder:** All assets on LitVM are testnet tokens with no monetary value. Use Addax to test strategies and integrations risk-free.
{% endtab %}
{% endtabs %}
