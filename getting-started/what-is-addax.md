# What is Addax

Addax is a decentralized exchange on LitVM LiteForge built around two core components:

**Concentrated Liquidity Pools (V3 model)** — Liquidity providers deposit tokens into pools and earn fees from swaps. Unlike traditional AMMs where liquidity is spread uniformly across all prices, Addax lets LPs set a price range. Liquidity only activates when the market price is inside that range, which means the same capital generates more fees compared to a passive position.

**ARIS** — An intent layer that lets users sign orders offchain. A network of fillers watches for open orders and executes them onchain when conditions are met. This supports limit orders, dutch auctions, and automated DCA schedules without users needing to be online.

## How a swap works

1. User selects a token pair and amount on [addax.finance](https://addax.finance).
2. The aggregator queries all available routes — Addax pools at each fee tier and external DEX venues on LitVM.
3. The best net output is selected and the swap is executed through `AddaxV3SwapRouter` in a single transaction.
4. The LP who provided liquidity in the active range earns the swap fee on that trade.

## How an ARIS order works

1. User signs an EIP-712 typed order offchain — no gas required to place the order.
2. The signed order is submitted to the Addax order API.
3. Fillers poll the API, decode open orders, and call `execute()` on the relevant reactor contract when conditions are satisfied.
4. The reactor validates the signature, checks timing and constraints, transfers input tokens from the user via Permit2, and delivers outputs to the recipient.
