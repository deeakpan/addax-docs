# Contracts & Addresses

All contracts are deployed on LitVM LiteForge (chain ID 4441). Source code is available at [github.com/deeakpan/addax/tree/main/contracts](https://github.com/deeakpan/addax/tree/main/contracts).

## Testnet Deployments

| Contract | Address | Source |
|---|---|---|
| AddaxV3Factory | `0x8b84D582622DfF7bC3582365941791B68Ae70f4d` | [UniswapV3Factory](https://github.com/deeakpan/addax/tree/main/contracts/v3-core) |
| AddaxV3SwapRouter | `0xc6B464b146B1C9D03646b624f6B3ECA9794FdfB5` | [SwapRouter](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3QuoterV2 | `0x46D5676250100e6f9befF90455E032F85cC8775a` | [QuoterV2](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3NonfungiblePositionManager | `0x049293EcBc8771488aD16EeDE035e14342D60D9F` | [NonfungiblePositionManager](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3TickLens | `0x942108E5AE08De7A48e1F28c3F96BB8849d472b6` | [TickLens](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3InterfaceMulticall | `0xD1Db44b4f9CDa05C32C7E9d1896296837a1cee57` | [UniswapInterfaceMulticall](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxAggregatorV3 | `0x0b0935E14b72D0d20c477788d7BD70D89632BEd5` | [AddaxAggregatorV3](https://github.com/deeakpan/addax/tree/main/contracts/src/addax) |
| wzkLTC | `0x6eF676c26E8C977554DA186eD0B215956E8F8753` | [WzkLTC](https://github.com/deeakpan/addax/tree/main/contracts/src/addax) |
| aUSDC | `0x72F4efAC9133d28fa05aEbc9edCd8fC3dE14BB50` | [AUSDC](https://github.com/deeakpan/addax/tree/main/contracts/src/addax) |
| wzkLTC (Legacy) | `0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea` | — |
| USDC (Legacy) | `0xd5118dEe968d1533B2A57aB66C266010AD8957fa` | — |
| ArisPermit2 | `0xE6c670CED4e06e088AEb7141E0A0a557da517DbB` | [Permit2](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/permit2) |
| ArisLimitOrderReactor | `0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502` | [LimitOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisDutchOrderReactor | `0xA70c28ef82d1427Fa68F65960FbfFb1198349593` | [DutchOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisExclusiveDutchOrderReactor | `0x538048AD8cC03Fe083CC5414a705D14ac0561331` | [ExclusiveDutchOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisDcaOrderReactor | `0x99D007fD09211B437Ecc415bB8D08Cc68b877672` | [DcaOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |

## Contract roles

**AddaxV3Factory** — Creates pools. Call `createPool(tokenA, tokenB, fee)` to deploy a new pool. The factory is permanent and not upgradeable.

**AddaxV3SwapRouter** — The primary swap entry point. Supports `exactInputSingle`, `exactInput` (multi-hop), `exactOutputSingle`, and `exactOutput`. Also exposes a `multicall` to batch swap + unwrap + refund in one transaction.

**AddaxV3QuoterV2** — Read-only quoter. Use this offchain to simulate swap output before executing. Never call this onchain.

**AddaxV3NonfungiblePositionManager** — Mint, increase, decrease, and collect LP positions. Each position is an ERC-721 NFT.

**AddaxAggregatorV3** — Routes swaps through any allowlisted router. Supports all four swap types and multicall batching. New routers (e.g. external DEXes) are added by the Addax team via `addRouter`.

**ArisPermit2** — Handles token approvals for all ARIS reactor settlements. Users approve Permit2 once; reactors use permit witness signatures for individual order fills.
