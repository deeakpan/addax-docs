# Collateral & Tokens

Addax lets you post margin in three collaterals. Each collateral has its own **stack** (trading contracts + gToken vault), so a USDC trade is backed by the gUSDC vault, an ADDX trade by the gADDX vault, and a zkLTC trade by the gzKLTC vault.

## Supported collaterals

| Collateral | Symbol | Decimals | ERC-20 address | Vault |
|---|---|---|---|---|
| USDC | `USDC` | 6 | `0xA6b7A782Fc4349914dADde5b8A8A8B1daDFBF6DB` | gUSDC |
| ADDX | `ADDX` | 18 | `0xf6078Be81aBCC95BAC306356963E7adB986783f9` | gADDX |
| Wrapped zkLTC | `WzkLTC` | 18 | `0xA52F83592b9216C574e11324d4468F078aEA05BE` | gzKLTC |

> Contract addresses can be overridden per-deployment via `NEXT_PUBLIC_PERPS_*` environment variables. The values above are the current LiteForge deployment. See [Contracts & Addresses](../protocol/contracts.md) for the full list.

## USDC

The primary trading collateral, a 6-decimal test USDC with a faucet. Deposit USDC as margin and your position is backed by the **gUSDC** vault. This is the default and recommended collateral for most traders.

## ADDX

The native Addax protocol token (18 decimals). Trades collateralized in ADDX are backed by the **gADDX** vault. ADDX can also be staked — see [ADDX Staking](../vaults/staking.md).

## zkLTC and WzkLTC

`zkLTC` is the native gas token on LiteForge. To use it as margin it must be **wrapped** to the ERC-20 `WzkLTC`. The app wraps it for you when you select zkLTC as collateral; you can also wrap manually:

```solidity
// Wrap native zkLTC into WzkLTC (WETH9 interface)
WzkLTC.deposit{ value: 1 ether }();

// Unwrap back to native zkLTC
WzkLTC.withdraw(amount);
```

Trades collateralized in WzkLTC are backed by the **gzKLTC** vault.

## Getting testnet tokens

- **zkLTC** — claim from the LiteForge faucet (see [Get Testnet zkLTC](faucet.md)).
- **USDC** — mint from the in-app faucet / test USDC contract.
- **ADDX** — obtain from the app faucet or a distribution as available on testnet.

Continue to [Setting Up to Trade](setting-up-to-trade.md).
