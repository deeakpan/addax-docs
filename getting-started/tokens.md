# Tokens

## wzkLTC — Addax Wrapped zkLTC

`0x6eF676c26E8C977554DA186eD0B215956E8F8753`

The primary wrapped native token used across all Addax pools. It follows the WETH9 interface — deposit native zkLTC to receive wzkLTC 1:1, burn wzkLTC to withdraw native.

**Additional deposit path:** If you hold legacy wzkLTC (`0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea`), you can convert it to Addax wzkLTC 1:1 by approving the new contract and calling `depositWrapped(amount)`. No native zkLTC required.

```solidity
// Wrap native zkLTC
wzkLTC.deposit{ value: 1 ether }();

// Wrap legacy wzkLTC (approve first)
legacyWzkLTC.approve(wzkLTCAddress, amount);
wzkLTC.depositWrapped(amount);

// Unwrap to native
wzkLTC.withdraw(amount);

// Unwrap to legacy wzkLTC
wzkLTC.withdrawWrapped(amount);
```

## Legacy wzkLTC

`0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea`

The original wrapped zkLTC from the previous deployment. Still valid onchain but no longer used as the pool base asset on Addax. Convert to the new wzkLTC via `depositWrapped`.

## USDC

The Addax quote asset is **aUSDC** (`0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50`, 6 decimals), a 1:1 wrapper over legacy USDC (`0xd5118dEe968d1533B2A57aB66C266010AD8957fa`).  
Convert with `depositUnderlying(amount)` after approving legacy USDC to the aUSDC contract.
