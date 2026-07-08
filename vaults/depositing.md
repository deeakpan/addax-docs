# Depositing & Withdrawing

gToken vaults follow an ERC-4626-style deposit/withdraw model. You deposit the underlying collateral and receive gToken shares; you redeem gTokens to get the underlying back, including your share of accrued fees and trader PnL.

## Depositing

1. Go to the **Vaults** page and pick a vault (gUSDC, gzKLTC, or gADDX).
2. Enter the amount of the underlying asset to deposit.
3. **Approve** the vault to spend your tokens (one-time per token).
   - For the gzKLTC vault you can deposit **native zkLTC** (wrapped automatically) or **WzkLTC** directly.
4. Confirm the deposit. You receive gTokens representing your share of the vault.

```solidity
// ERC-4626-style deposit
gToken.deposit(assets, receiver); // returns shares minted
```

## What your gTokens are worth

Your gToken balance is fixed, but redemption value changes with the vault's **assets per share**:

```
withdrawable = shares × (totalAssets / totalSupply)
```

As the vault collects fees and trader losses, assets per share rises; when traders win, it can fall.

## Withdrawing

1. On the Vaults page, choose **Withdraw** and enter a gToken (share) amount or an asset amount.
2. Confirm the transaction to redeem your shares for the underlying asset.

```solidity
// Redeem shares for underlying
gToken.redeem(shares, receiver, owner); // returns assets returned
// or withdraw an exact asset amount
gToken.withdraw(assets, receiver, owner);
```

## Withdrawal timelocks

To protect the vault from bank-run dynamics and to keep enough collateral available for trader settlement, vaults may enforce **epoch-based withdrawal windows or timelocks**. If a timelock applies, you'll request a withdrawal and complete it after the unlock period. The app shows any active lock and the amount currently withdrawable.

## Contract addresses

See [Contracts & Addresses](../protocol/contracts.md) for each vault's address.
