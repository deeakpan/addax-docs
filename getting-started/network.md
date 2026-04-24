# Network & Setup

Addax is deployed on **LitVM LiteForge** — a testnet chain built on the LitVM stack.

## Network details

| Parameter | Value |
|---|---|
| Network name | LitVM LiteForge |
| Chain ID | `4441` |
| RPC (HTTP) | `https://liteforge.rpc.caldera.xyz/http` |
| RPC (WebSocket) | `wss://liteforge.rpc.caldera.xyz/ws` |
| Block explorer | `https://liteforge.explorer.caldera.xyz` |
| Native gas token | `zkLTC` |

## Adding to MetaMask

Open MetaMask → Settings → Networks → Add Network, and fill in the values above.

## Getting testnet tokens

- **zkLTC** — native gas token, available from the LitVM faucet.
- **USDC** — mintable test token on the network.
- **wzkLTC** — wrap native zkLTC via the Addax interface or call `deposit()` on the wzkLTC contract directly.
