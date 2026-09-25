# Network & Setup

{% tabs %}
{% tab title="Mainnet" %}
Mainnet trading settles on Ethereum, through Lighter. You stay in the Addax app. The address that receives USDC is your embedded wallet.

| | |
|---|---|
| Settlement | Ethereum |
| Collateral | USDC |
| Trading venue | Lighter |
| Addax fee | Up to 0.1% of trade size |

## Bridges

USDC is delivered to your embedded wallet, then deposited for you.

| Source | Status |
|---|---|
| Ethereum USDC | Supported |
| Circle CCTP from Arbitrum, Base, Optimism, Polygon, and Avalanche | Supported |
| LitVM | Planned. The destination is locked to your embedded wallet. |

The LitVM route is not live. It is included so the destination rule is fixed before launch: the bridge cannot send funds to a different address.

## What you approve

You approve two things, once each:

1. A policy that lets Addax pull a chosen amount of USDC from your embedded wallet and deposit it under your address.
2. Addax as a partner, which caps the trading fee at 0.1%.

After the deposit is credited, you register an API key. Trades use that key.
{% endtab %}

{% tab title="Testnet" %}
Addax is deployed on **LitVM**.

## Network details

| Parameter | Value |
|---|---|
| Network name | LitVM |
| Chain ID | `4441` |
| RPC (HTTP) | `https://liteforge.rpc.caldera.xyz/http` |
| RPC (WebSocket) | `wss://liteforge.rpc.caldera.xyz/ws` |
| Block explorer | `https://liteforge.explorer.caldera.xyz` |
| Native gas token | `zkLTC` |

## Adding to MetaMask

Open MetaMask -> Settings -> Networks -> Add Network, and fill in the values above. Or connect your wallet at the Addax app and approve the "Add network" prompt automatically.

## What you need to trade

1. **zkLTC** for gas, claim it from the LitVM faucet (see [Get Testnet zkLTC](faucet.md)).
2. **Collateral**: USDC, ADDX, or zkLTC to use as margin. See [Collateral & Tokens](tokens.md).

Once your wallet is connected and funded, head to [Setting Up to Trade](setting-up-to-trade.md).
{% endtab %}
{% endtabs %}
