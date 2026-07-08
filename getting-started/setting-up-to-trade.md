# Setting Up to Trade

Follow these steps to place your first trade on Addax.

## 1. Connect your wallet

Open the Addax app and connect an EVM wallet (MetaMask, Rabby, WalletConnect, etc.). Approve the prompt to add or switch to the **LitVM LiteForge** network (chain ID `4441`).

## 2. Get gas

Every transaction costs a small amount of native **zkLTC** for gas. Claim testnet zkLTC from the LiteForge faucet — see [Get Testnet zkLTC](faucet.md).

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
3. Set your **collateral amount** and **leverage** (1x–150x).
4. Optionally set a **limit price**, **take-profit**, and **stop-loss**.
5. Confirm the transaction.

For a full walkthrough, continue to [Opening & Closing Trades](../trading/opening-closing-trades.md).

> **Testnet reminder:** All assets on LiteForge are testnet tokens with no monetary value. Use Addax to test strategies and integrations risk-free.
