# Contracts & Addresses

All contracts are deployed on LitVM LiteForge (chain ID 4441). Source code is available at [github.com/deeakpan/addax/tree/main/contracts](https://github.com/deeakpan/addax/tree/main/contracts).

## Testnet Deployments

| Contract | Address | Source |
|---|---|---|
| AddaxV3Factory | `0x8E15B5482B201209300805A8eCD09cD1dA5800cc` | [UniswapV3Factory](https://github.com/deeakpan/addax/tree/main/contracts/v3-core) |
| AddaxV3SwapRouter | `0xD3B3b516d159AC5D0ddB579AA6feD92598202B55` | [SwapRouter](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3QuoterV2 | `0x3Fb614d43146792FC0EfAe84f076e24085FD73A2` | [QuoterV2](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3NonfungiblePositionManager | `0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d` | [NonfungiblePositionManager](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3TickLens | `0x1Af35402EbA8c8e34Ed13c106d27DDe56F71aBf9` | [TickLens](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxV3InterfaceMulticall | `0xa26dAd7BCDbECA5fE6E3a5457ce6BB9988697D0b` | [UniswapInterfaceMulticall](https://github.com/deeakpan/addax/tree/main/contracts) |
| AddaxAggregatorV3 | `0x7dB9ac619cE9A82ffd9c26fbb2Dc5BeD0215E2d6` | [AddaxAggregatorV3](https://github.com/deeakpan/addax/tree/main/contracts/src/addax) |
| wzkLTC | `0xe8Af2359E1E3E034931595F4a17F0fD4d8655Cee` | [WzkLTC](https://github.com/deeakpan/addax/tree/main/contracts/src/addax) |
| wzkLTC (Legacy) | `0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea` | — |
| ArisPermit2 | `0xE6c670CED4e06e088AEb7141E0A0a557da517DbB` | [Permit2](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/permit2) |
| ArisLimitOrderReactor | `0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502` | [LimitOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisDutchOrderReactor | `0xA70c28ef82d1427Fa68F65960FbfFb1198349593` | [DutchOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisExclusiveDutchOrderReactor | `0x538048AD8cC03Fe083CC5414a705D14ac0561331` | [ExclusiveDutchOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |
| ArisDcaOrderReactor | `0x99D007fD09211B437Ecc415bB8D08Cc68b877672` | [DcaOrderReactor](https://github.com/deeakpan/addax/tree/main/contracts/src/aris/arisCore/reactors) |

## Contract roles

**AddaxV3Factory** — Creates pools. Call `createPool(tokenA, tokenB, fee)` to deploy a new pool. The factory is permanent and not upgradeable.

**AddaxV3SwapRouter** — The primary swap entry point. Supports `exactInputSingle`, `exactInput` (multi-hop), `exactOutputSingle`, and `exactOutput`. Also exposes a `multicall` to batch swap + unwrap + refund in one transaction.

**AddaxV3QuoterV2** — Read-only quoter. Use this off-chain to simulate swap output before executing. Never call this on-chain.

**AddaxV3NonfungiblePositionManager** — Mint, increase, decrease, and collect LP positions. Each position is an ERC-721 NFT.

**AddaxAggregatorV3** — Routes swaps through any allowlisted router. Supports all four swap types and multicall batching. New routers (e.g. external DEXes) are added by the Addax team via `addRouter`.

**ArisPermit2** — Handles token approvals for all ARIS reactor settlements. Users approve Permit2 once; reactors use permit witness signatures for individual order fills.
