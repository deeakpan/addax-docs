# Contracts Overview

Addax uses a set of pool/periphery contracts and ARIS reactor contracts deployed on LitVM LiteForge (chain ID 4441).

## Deployed Addresses

### Swap Stack

| Contract | Address |
|---|---|
| AddaxV3Factory | `0x8E15B5482B201209300805A8eCD09cD1dA5800cc` |
| AddaxV3SwapRouter | `0xD3B3b516d159AC5D0ddB579AA6feD92598202B55` |
| AddaxV3QuoterV2 | `0x3Fb614d43146792FC0EfAe84f076e24085FD73A2` |
| AddaxV3NonfungiblePositionManager | `0xc2f05acF91A95a410BE2abb9e779e3CFA524C01d` |
| AddaxV3TickLens | `0x1Af35402EbA8c8e34Ed13c106d27DDe56F71aBf9` |
| AddaxV3InterfaceMulticall | `0xa26dAd7BCDbECA5fE6E3a5457ce6BB9988697D0b` |

### Tokens

| Contract | Address |
|---|---|
| wzkLTC (Addax Wrapped zkLTC) | `0xe8Af2359E1E3E034931595F4a17F0fD4d8655Cee` |
| wzkLTC Legacy | `0x60A84eBC3483fEFB251B76Aea5B8458026Ef4bea` |

### ARIS Stack

| Contract | Address |
|---|---|
| ArisPermit2 | `0xE6c670CED4e06e088AEb7141E0A0a557da517DbB` |
| ArisDutchOrderReactor | `0xA70c28ef82d1427Fa68F65960FbfFb1198349593` |
| ArisExclusiveDutchOrderReactor | `0x538048AD8cC03Fe083CC5414a705D14ac0561331` |
| ArisLimitOrderReactor | `0x40ae8eCfc8e99D21634e8061C4E9CCf8da308502` |
| ArisDcaOrderReactor | `0x99D007fD09211B437Ecc415bB8D08Cc68b877672` |

### Aggregator

| Contract | Address |
|---|---|
| AddaxAggregatorV3 | `0x7dB9ac619cE9A82ffd9c26fbb2Dc5BeD0215E2d6` |

## Main contract groups

- **Swap stack** — Factory, SwapRouter, Quoter, Position Manager (NFT LP positions)
- **ARIS stack** — Permit2, Limit reactor, DCA reactor, Dutch/exclusive reactors
- **Aggregator** — Routes swaps across multiple DEXes in one transaction

## Operational pattern

- User signs typed data (witness) offchain.
- Filler/relayer submits onchain settlement.
- Reactor validates witness + timing/constraints.
- Settlement transfers are executed through Permit2 and reactor logic.
