# Order Types

| Type | What it does |
|---|---|
| **Market** | Matches the book now, at your worst price or better |
| **Limit** | Rests on the book at your price |
| **Stop loss** | Triggers, then sends a market order |
| **Stop loss limit** | Triggers, then rests a limit order |
| **Take profit** | Triggers, then sends a market order |
| **Take profit limit** | Triggers, then rests a limit order |
| **TWAP** | Breaks the order into smaller fills over a window |

The price on a market order is the worst price you will accept. If the book cannot fill you there or better, the order is canceled.

Addax is built on [Lighter](https://lighter.xyz/).
