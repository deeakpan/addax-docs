# Get Testnet zkLTC

`zkLTC` is the native gas token on LitVM. You need a small amount to pay for transactions (opening/closing trades, approvals, vault deposits).

## Claim from the faucet

Go to the LitVM faucet at [liteforge.hub.caldera.xyz](https://liteforge.hub.caldera.xyz/), connect your wallet, and request testnet zkLTC.

## Using zkLTC as collateral

Gas aside, you can also trade with zkLTC as margin. Because margin must be an ERC-20, native zkLTC is wrapped into **WzkLTC** first. The Addax app does this automatically when you pick zkLTC as collateral, or you can wrap it yourself:

```typescript
const wzkLTC = new ethers.Contract(
 "0xA52F83592b9216C574e11324d4468F078aEA05BE",
 ["function deposit() payable", "function withdraw(uint256)"],
 signer
);

// Wrap 1 zkLTC -> 1 WzkLTC
await wzkLTC.deposit({ value: ethers.parseEther("1") });
```

## Getting trading collateral

You'll also want collateral to trade with. USDC and ADDX are available from the in-app faucet on testnet. See [Collateral & Tokens](tokens.md) for details and addresses.
