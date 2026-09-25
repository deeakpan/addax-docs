# Network & Setup

{% tabs %}

{% tab title="Mainnet" %}

Mainnet trading uses an order book. Margin is USDC. Each market has its own leverage cap. Major crypto markets allow up to 50x.

The Addax fee is up to 0.1% of the filled size.

Addax is built on [Lighter](https://lighter.xyz/).

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
