# Glossary

Key terms used across the Addax documentation.

**Addax** — A decentralized leveraged trading platform on LitVM LiteForge, built on the gTrade / Gains Network architecture.

**Collateral / Margin** — The tokens you post to open a position (USDC, ADDX, or WzkLTC). Your maximum loss on a position is bounded by its margin.

**Leverage** — Multiplier applied to your collateral to size a position (1x–150x). Position size = collateral × leverage.

**Position size / Notional** — The USD-denominated exposure of a trade (collateral × leverage). Fees are charged on notional.

**Long / Short** — A long profits when price rises; a short profits when price falls.

**Mark price** — The oracle-provided price used to value positions and settle trades.

**Spread** — A per-pair adjustment applied around the mark price so longs open slightly above and shorts slightly below mid; compensates the vault.

**Price impact** — A size- and skew-dependent adjustment to execution price that keeps long/short open interest balanced.

**Borrowing / Holding fee** — A time-based fee accrued while a position is open, based on open-interest skew; erodes margin over time.

**Take-profit (TP)** — An automatic close order that locks in profit at a target price.

**Stop-loss (SL)** — An automatic close order that caps loss at a chosen price.

**Limit order** — An order to open a position only when price reaches a specified level; stored on-chain until triggered.

**Liquidation** — Forced close of a position when losses plus fees consume its margin down to the maintenance threshold.

**Liquidation price** — The market price at which a position would be liquidated.

**Keeper** — An off-chain bot that executes triggers (limit, TP, SL, liquidation) and pushes oracle prices, earning rewards.

**Trigger keeper** — The keeper that scans positions/orders and executes OPEN/TP/SL/LIQ triggers.

**Oracle keeper** — The keeper that keeps the price feed fresh.

**executeNftOrder** — The Trading contract call a keeper uses to initiate a pending trigger (limit/TP/SL/LIQ); emits `NftOrderInitiated`.

**fulfillOrder** — The Price Aggregator call that resolves the price and completes an initiated order.

**gToken / Vault** — ERC-4626-style liquidity vaults (gUSDC, gzKLTC, gADDX) that act as counterparty to all trades and pay LPs.

**Stack** — A full set of trading contracts (Trading, Storage, Callbacks, Vault, Pair Infos, Price Aggregator) for one collateral.

**Pair Infos** — The contract holding per-pair parameters: spread, fees, borrowing rates, and leverage caps.

**pairIndex** — The on-chain integer identifier for a market (e.g. BTC = 0).

**Oracle key** — The feed identifier for a market (e.g. `BTC/USD`) used by the DIA oracle / price aggregator.

**DIA Oracle** — The underlying price-feed contract the aggregator reads.

**ADDX** — The native Addax protocol token; usable as collateral and stakeable for rewards.

**zkLTC / WzkLTC** — The native LiteForge gas token and its wrapped ERC-20 form (used as margin).

**LitVM LiteForge** — The testnet chain (ID 4441) Addax is deployed on.
