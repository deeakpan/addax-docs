# Get Testnet zkLTC

zkLTC is the native gas token on LitVM LiteForge. You need it to pay for transactions.

## Claim from the faucet

Go to the LiteForge faucet at [liteforge.hub.caldera.xyz](https://liteforge.hub.caldera.xyz/), connect your wallet, and request testnet zkLTC.

Once you have zkLTC you can wrap it to wzkLTC directly on [addax.finance](https://addax.finance) or by calling `deposit()` on the wzkLTC contract:

```typescript
const wzkLTC = new ethers.Contract(
  "0x6eF676c26E8C977554DA186eD0B215956E8F8753",
  ["function deposit() payable"],
  signer
);

await wzkLTC.deposit({ value: ethers.parseEther("1") });
```

If you already hold legacy wzkLTC (`0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea`) from a previous deployment, convert it 1:1 without needing native zkLTC:

```typescript
const legacy = new ethers.Contract(legacyAddress, ["function approve(address,uint256) returns (bool)"], signer);
await legacy.approve("0x6eF676c26E8C977554DA186eD0B215956E8F8753", amount);
await wzkLTC.depositWrapped(amount);
```

To convert legacy USDC to Addax aUSDC (1:1):

```typescript
const ausdc = new ethers.Contract(
  "0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50",
  ["function depositUnderlying(uint256)"],
  signer
);
await legacyUsdc.approve("0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50", amount);
await ausdc.depositUnderlying(amount);
```
