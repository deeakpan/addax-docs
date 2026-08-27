# FAQ

## What is Addax?

A decentralized leveraged trading platform on LitVM. Trade crypto, commodities, and equities with up to 100x leverage from your own wallet. Trades settle synthetically against a collateral vault at the oracle mark price. See [What is Addax](../getting-started/what-is-addax.md).

## Is this real money?

No. Addax is currently on the **LitVM testnet**. All tokens (zkLTC, USDC, ADDX) are testnet assets with no monetary value.

## Do I need an account?

No. Connect any EVM wallet, add the LitVM network, fund it with testnet tokens, and trade. See [Setting Up to Trade](../getting-started/setting-up-to-trade.md).

## What can I use as collateral?

USDC, ADDX, or zkLTC (wrapped to WzkLTC). Each maps to its own vault stack, gUSDC, gADDX, gzKLTC. See [Collateral & Tokens](../getting-started/tokens.md).

## What's the maximum leverage?

Up to **100x**, depending on the market. Crypto majors allow the highest leverage; commodities and equities are typically capped lower. See [Leverage & Liquidation](../trading/leverage-and-liquidation.md).

## How is my entry price determined?

From the **oracle mark price** at execution, adjusted by the pair's spread (and price impact for large size). There is no order-book slippage. See [Price Oracle](../protocol/price-oracle.md).

## What order types are supported?

Market and limit orders to open, plus take-profit and stop-loss to close. Liquidation is the protocol's automatic close trigger. See [Order Types](../trading/order-types.md).

## Do my limit / TP / SL orders work when I'm offline?

Yes. They're stored on-chain and executed by **keepers** when conditions are met, your wallet doesn't need to be online. See [Keepers](../protocol/keepers.md).

## When do I get liquidated?

When accumulated losses plus fees erode your margin down to the maintenance threshold. Use a stop-loss and lower leverage to reduce the risk. See [Leverage & Liquidation](../trading/leverage-and-liquidation.md).

## What fees do I pay?

Spread, an opening and closing fee on notional, price impact on large size, and **funding** while the market is skewed. See [Fees & Spread](../trading/fees-and-spread.md) and [Funding Rates](../trading/funding-rates.md).

## How does funding work?

If one side of a market has more open interest, that side pays the other. Balanced markets pay little or nothing. Funding accrues into your position over time — see [Funding Rates](../trading/funding-rates.md).

## How do I earn as a liquidity provider?

Deposit into a gToken vault (gUSDC, gzKLTC, gADDX). The vault is the counterparty to traders and earns from fees and net trader losses; your gTokens grow in redemption value over time. See [gToken Vaults](../vaults/overview.md).

## Can I run a keeper?

Yes, keeper execution is permissionless and rewarded. You need a wallet holding the keeper NFT. See [Building Keeper Bots](../developers/building-bots.md).

## Where are the contract addresses?

On the [Contracts & Addresses](../protocol/contracts.md) page.
