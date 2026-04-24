# Protocol Overview

Addax is built on a Uniswap V3 core — one of the most battle-tested AMM codebases in DeFi, securing billions in liquidity across hundreds of deployments. The factory, pool, and periphery contracts follow the same architecture with a custom pool init hash and native token wrapper for LitVM.

On top of the swap infrastructure sits ARIS, a reactor-based intent execution layer. ARIS reactors inherit from a shared `BaseReactor` which handles signature verification, Permit2 token transfers, fee injection, and output delivery. Each order type (limit, dutch, DCA) is a separate reactor that implements its own resolution and timing logic.

## Two layers

**Swap layer** — permissionless pool creation, concentrated liquidity, NFT LP positions. Any two tokens can form a pool at any fee tier. No governance required.

**Intent layer (ARIS)** — offchain order signing, onchain settlement. Users sign typed orders that expire by deadline and are protected by Permit2 nonces. Fillers compete to execute them.

## What makes the contracts safe

The V3 pool contracts have been formally reviewed and battle-tested in production since 2021. The ARIS reactor contracts are modelled on the Uniswap X reactor architecture with the following protections built in:

- Reentrancy guards on all execution entry points
- Permit2 nonce-based replay protection per order
- Reactor address bound into every signed order (cross-reactor replay is impossible)
- Protocol fee capped onchain at 0.05% maximum, enforced by `ProtocolFees.sol`
- Deadline enforced at the reactor level before any token transfer occurs

## Source code

All contracts are open source at [github.com/deeakpan/addax/tree/main/contracts](https://github.com/deeakpan/addax/tree/main/contracts).
