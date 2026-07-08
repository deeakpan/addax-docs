# Opening & Closing Trades

This is a step-by-step guide to opening, managing, and closing positions on Addax.

## Opening a trade

1. **Select a market** — pick a pair such as BTC, ETH, LTC, XAU, or TSLA from the market selector.
2. **Choose direction** — **Long** if you expect the price to rise, **Short** if you expect it to fall.
3. **Pick collateral** — USDC, ADDX, or zkLTC. Each maps to its own vault stack.
4. **Enter amount & leverage** — set your collateral amount and slide leverage from 1x to 150x. Position size = collateral × leverage.
5. **Choose order type:**
   - **Market** — opens immediately at the current oracle price.
   - **Limit** — opens only when the market reaches your specified price. When you switch to Limit, the price field is pre-filled with the current mark price; edit it to your target.
6. **(Optional) Set TP / SL** — attach a take-profit and/or stop-loss so the position closes automatically.
7. **Confirm** — sign the transaction. Market orders open in the same transaction once the price is confirmed; limit orders are stored on-chain until triggered.

## Managing an open position

From the **Positions** tab you can:

- **Add / edit take-profit and stop-loss** on a live position.
- **Partially close** to realize part of your PnL and reduce size.
- **Close fully** at the current market price.

Pending limit orders appear under the **Open Orders / Limits** tab, where you can **cancel** them before they trigger.

## Closing a trade

- **Manual close** — hit Close (full or partial); the position settles at the current oracle price minus spread.
- **Take-profit** — a keeper closes the position when price reaches your TP target.
- **Stop-loss** — a keeper closes the position when price hits your SL, capping your loss.
- **Liquidation** — if losses erode your margin past the maintenance threshold, a keeper liquidates the position. See [Leverage & Liquidation](leverage-and-liquidation.md).

## Who executes triggers

Limit opens, TP, SL, and liquidations are executed by **keeper bots**, not by you. Keepers watch the chain and call the trigger + fulfill functions on the contracts when conditions are met, earning a small reward. This means your TP/SL/limit orders work even when your wallet is offline. See [Keepers](../protocol/keepers.md).
