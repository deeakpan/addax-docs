# Contracts & Addresses

Addax is deployed on **LitVM LiteForge** (chain ID `4441`). Each collateral has its own **stack** of contracts. Addresses below are the current LiteForge deployment; the app can override them per-environment via `NEXT_PUBLIC_PERPS_*` variables.

## Network

| | |
|---|---|
| Chain ID | `4441` |
| RPC | `https://liteforge.rpc.caldera.xyz/http` |
| Explorer | `https://liteforge.explorer.caldera.xyz` |
| Deploy block (indexing) | `24989478` |

## Tokens

| Token | Address | Decimals |
|---|---|---|
| USDC | `0xA6b7A782Fc4349914dADde5b8A8A8B1daDFBF6DB` | 6 |
| ADDX | `0xf6078Be81aBCC95BAC306356963E7adB986783f9` | 18 |
| WzkLTC | `0xA52F83592b9216C574e11324d4468F078aEA05BE` | 18 |

## gUSDC stack (primary)

| Contract | Address |
|---|---|
| Storage | `0xe4D06BeE42d9B8CbF3d95520dCC2E522bF67D2a3` |
| Trading | `0x6F23a6500a211F1fC9684672C70952C8a6221b1B` |
| Callbacks | `0x62920E443abFEFb91126FAC8F4D2e1Db60aA5341` |
| Pair Infos | `0xc491c999eB5734eCFBe0E3AAc32DED28CA3259B7` |
| Price Aggregator | `0x3f0fA5CeC8B45111777baB68E37155ca0edC6400` |
| Vault (gUSDC) | `0x1e40483dee13E72c6E9d412989D408f00e75dbEF` |
| DIA Oracle | `0xFf856a958eFA7965A4dFC2BFb09dDbc9EABe9aAb` |
| Staking | `0xA0961eE578d708837225012fb13b7b6f0dF5Ec15` |

## gzKLTC stack

| Contract | Address |
|---|---|
| Storage | `0x0bab0C9354392989a20Ad3D739Bd1dE30cC82B1F` |
| Trading | `0x63Ea6b78E663a296b1A86A9Db684F008aCD88f01` |
| Callbacks | `0x17D04b545b80deA3F60295C70Fb35485aB7bac68` |
| Pair Infos | `0x93Fd0177f0b03891D5A114E261DDA53fB8b7b032` |
| Price Aggregator | `0x182A16a127cfD8a1b4EaE0a5653df467B3E6456E` |
| Vault (gzKLTC) | `0xBE154C61Ddb666Da4B9c63d56E21D7E6f7355FF4` |

## gADDX stack

| Contract | Address |
|---|---|
| Storage | `0xfa63B98b6feEBD766d8863b25d2dc0d5c668b8Ad` |
| Trading | `0x498771e39066926D424B7fB939D93F61bad3E040` |
| Callbacks | `0xe3E60155b444356C14E9A0088A1882a6F9430B49` |
| Pair Infos | `0xb82c0b371233588ee72c841eb6178D87d57F89Ce` |
| Price Aggregator | `0x61af9FBbEA6D011Cea8d1FE6c674f89bD8eF5D8F` |
| Vault (gADDX) | `0x634767abA7e823D1B995c40acC68389e1A8D642E` |

## Notes

- Use the **gUSDC stack** for most integrations — it is the primary UI and keeper stack.
- The **Trading** contract is the write entry point; **Storage** is the canonical read source; **Pair Infos** holds per-pair fee/leverage params. See [Trading Contracts](../developers/contracts.md).
- Always verify addresses against the live app configuration before sending funds; testnet deployments can be redeployed.
