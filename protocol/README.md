# Protocol

The Addax protocol consists of two layers deployed on LitVM LiteForge.

**V3 Swap Stack** — A Uniswap V3-compatible factory, router, quoter, and position manager. Pools are permissionlessly created between any two tokens at a chosen fee tier. LP positions are ERC-721 NFTs.

**ARIS** — The intent execution layer. Users sign typed orders offchain. Fillers submit them onchain to reactor contracts that validate constraints and settle transfers through Permit2.

## Sections

- [Contracts & Addresses](./contracts.md)
- [Pools](./pools.md)
- [Fees](./fees.md)
- [Aggregator](./aggregator.md)
