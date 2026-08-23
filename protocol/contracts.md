# Contracts & Addresses

Addax perpetuals are deployed on **LitVM** (chain ID `4441`). Each collateral has its own **stack**. Addresses below match the LitVM deployment manifest (`contracts/deployments/liteforge-gtrade.json`).

## Network

| | |
|---|---|
| Chain ID | `4441` |
| RPC | `https://liteforge.rpc.caldera.xyz/http` |
| Explorer | `https://liteforge.explorer.caldera.xyz` |
| Deploy block (indexing) | `38095768` |
| Deployed at | `2026-08-08` |

## Tokens

| Token | Address | Decimals |
|---|---|---|
| USDC (AddaxUSDC) | `0xA6b7A782Fc4349914dADde5b8A8A8B1daDFBF6DB` | 6 |
| ADDX | `0xf6078Be81aBCC95BAC306356963E7adB986783f9` | 18 |
| WzkLTC | `0xA52F83592b9216C574e11324d4468F078aEA05BE` | 18 |

## Oracle & feeds

| | Address |
|---|---|
| DIA oracle (testnet) | `0xEd7f45c29FE6676e1eB7096aD5D6966abd62Bd1a` |

**Testnet oracle policy:** DIA **push** feeds with **1%** deviation and a **1h** heartbeat (see [Price Oracle](price-oracle.md)). **Mainnet** is planned to use DIA **pull** delivery.

### Per-market feed adapters

| Market | Feed adapter |
|---|---|
| BTC | `0x1906AA2bAf29c79b8FDC604fF64be7e8A4030204` |
| ETH | `0x7a413cdc7005Ae2e3fD66483339B07ab4B0043d6` |
| LTC | `0x280CcB394DE0C1FAA988d0e066EbDA631eD3D41A` |
| XAU | `0xd2640f29cFaBD5521630bF4E68E5Cff8e31DC776` |
| TSLA | `0x8E307ac0512B10a7Dd2ADDfBc357f64CfC51cA78` |
| SPCX | `0x122B459B3255C09856Db46c55B5B8de2422E60f5` |
| SOL | `0x1ef45191447F216f5C0b81b2b6f41cb38292dd00` |

## Vaults (gTokens)

| Vault | Address |
|---|---|
| gUSDC | `0xbA68d137F6AaD10a7490DDb94bbd718f59b6A1C6` |
| gWzkLTC | `0x7Fc5B0Bd106e124B4fCC5a6add01224E63605ba8` |
| gADDX | `0xbF39Acc7f4CEe52d9C02fF66B388761Cb1F941bf` |

## gUSDC stack (primary)

| Contract | Address |
|---|---|
| Storage | `0xEEC2067f8a310B2b09f9b97eC4c5247250D2c712` |
| Trading | `0x468A8eFB014bc7784C3BD1F6F3a7cf7feB07B1e8` |
| Callbacks | `0xE698737D9C37F455F8afc3f1574B5f19CEDd086C` |
| Pair Infos | `0xa574dAE7EbF8cA56a9AC80932bFf9862C6D62FFC` |
| Pairs Storage | `0xDbE5E61408698356E2687B6504A0f7144a5C262c` |
| Price Aggregator | `0xA184242a075bEA7012Ce83BD86f3E56a9bc33A73` |
| Vault (gUSDC) | `0xbA68d137F6AaD10a7490DDb94bbd718f59b6A1C6` |
| NFT | `0xADeE0A7572D40cC110b900fFa24d6C0A96907f46` |
| NFT Rewards | `0xCb809166FfA8B50B736A838a5877B38B9CdB0806` |
| Pool | `0x2acc3625eCEb8A1C651Cb0ac783594cC054f2227` |
| Staking | `0xA0961eE578d708837225012fb13b7b6f0dF5Ec15` |

## gWzkLTC stack

| Contract | Address |
|---|---|
| Storage | `0x9251e3fCE75bdeB0425158de14d44d97be04036D` |
| Trading | `0x97E09a9533f8D6c701F4fa88a0d1130DdC1576a4` |
| Callbacks | `0x33e6A62527222b0329486e8f1a3Cd07Fe994Bc4a` |
| Pair Infos | `0xce9adD3D6872D9476BDEFAA343417B46e09a6F7b` |
| Pairs Storage | `0x3111D14C6df9f0d5600a2A532501cbd36410ce7F` |
| Price Aggregator | `0x9fADa5a5D2FA70Bd9D31bA72003b011D860Ed76c` |
| Vault (gWzkLTC) | `0x7Fc5B0Bd106e124B4fCC5a6add01224E63605ba8` |
| NFT Rewards | `0x325360Fe9bd3CE8e87adE61ECea308E3e4e234eb` |

## gADDX stack

| Contract | Address |
|---|---|
| Storage | `0xa030b6174aeC909c6C5c22e7A08BEDDE5C584EF4` |
| Trading | `0x81e33163684f95b67b6dB84bae3875B34704f386` |
| Callbacks | `0xB631665892b61864c0BC6098CAfE3C2F174326A9` |
| Pair Infos | `0x89e53e0Ad13090bf53fAf9A0DAf707C511A3c831` |
| Pairs Storage | `0xCD15A63a63C8f6C068BA171B01a833813278D986` |
| Price Aggregator | `0x2e9F4F9fBb5BF483e523C864ecf6614383A38Dbf` |
| Vault (gADDX) | `0xbF39Acc7f4CEe52d9C02fF66B388761Cb1F941bf` |
| NFT Rewards | `0xB4076F11684CFE2FEB01D0694b8b1c4046a45d52` |

## Notes

- Prefer the **gUSDC stack** for most integrations, it is the primary UI and keeper stack.
- **Trading** is the write entry point; **Storage** is the canonical read source; **Pair Infos** holds per-pair fee/leverage params.
- Testnet deployments can be redeployed; always confirm addresses against the live app or the deployment manifest before sending funds.
- Protocol event indexing is provided via **Goldsky** subgraphs (see [Architecture Overview](overview.md)).
