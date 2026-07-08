# gToken Vaults

gToken vaults are the liquidity backbone of Addax. Liquidity providers deposit collateral into a vault and receive **gTokens** (ERC-4626-style shares) in return. The vault acts as the counterparty to every trade in its stack: it pays out winning traders and collects from losing ones, plus a share of trading fees.

## The three vaults

Each collateral has its own vault and its own trading stack:

| Vault | Deposit asset | Decimals | Backs trades in |
|---|---|---|---|
| **gUSDC** | USDC | 6 | USDC-collateralized positions |
| **gzKLTC** | zkLTC / WzkLTC | 18 | zkLTC-collateralized positions |
| **gADDX** | ADDX | 18 | ADDX-collateralized positions |

> The gzKLTC vault accepts both native zkLTC (wrapped for you) and the WzkLTC ERC-20.

## How LPs earn

When you deposit, you mint gTokens whose value tracks the vault's assets per share:

- **Trading fees** — a portion of every open/close fee flows to the vault, increasing assets per share.
- **Trader PnL** — the vault is the counterparty to traders. When traders net lose, the vault gains; when traders net win, the vault pays out.

Your gToken balance stays constant, but each gToken becomes redeemable for more (or fewer) underlying tokens over time. Your return is the change in **assets per share** between deposit and withdrawal.

## Risk

LPs take the other side of aggregate trader flow. In periods where traders are net profitable, the vault's assets per share can decline. Vault mechanics (fees, borrowing fees, price impact, and open-interest limits) are designed to keep the vault net-positive over time, but returns are not guaranteed and can be negative.

## Getting started

- To deposit or withdraw, see [Depositing & Withdrawing](depositing.md).
- To stake ADDX for protocol rewards, see [ADDX Staking](staking.md).
