# Tokens

## wzkLTC — Addax Wrapped zkLTC

`0xe8Af2359E1E3E034931595F4a17F0fD4d8655Cee`

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

The primary quote asset for wzkLTC/USDC pools on Addax. 6 decimal places.
