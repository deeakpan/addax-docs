# Get Testnet zkLTC

zkLTC is the native gas token on LitVM LiteForge. You need it to pay for transactions.

## Claim from the faucet

Go to the LiteForge faucet at [liteforge.hub.caldera.xyz](https://liteforge.hub.caldera.xyz/), connect your wallet, and request testnet zkLTC.

Once you have zkLTC you can wrap it to wzkLTC directly on [addax.finance](https://addax.finance) or by calling `deposit()` on the wzkLTC contract:

```typescript
const wzkLTC = new ethers.Contract(
  "0xe8Af2359E1E3E034931595F4a17F0fD4d8655Cee",
  ["function deposit() payable"],
  signer
);

await wzkLTC.deposit({ value: ethers.parseEther("1") });
```

If you already hold legacy wzkLTC (`0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea`) from a previous deployment, convert it 1:1 without needing native zkLTC:

```typescript
const legacy = new ethers.Contract(legacyAddress, ["function approve(address,uint256) returns (bool)"], signer);
await legacy.approve("0xe8Af2359E1E3E034931595F4a17F0fD4d8655Cee", amount);
await wzkLTC.depositWrapped(amount);
```
