# Trading

Trading on Addax is route-based execution across available liquidity.

## Swap flow

1. Choose sell and buy assets.
2. Add amount.
3. Addax computes candidate routes.
4. The best route is selected (highest out / lowest effective cost).
5. User signs and submits transaction.

## Key terms

- **Price impact**: difference between ideal and execution price due to finite liquidity.
- **Slippage tolerance**: minimum acceptable output protection.
- **Route**: one-hop or multi-hop path through pools.

## Advanced trading

- **Limit orders** via ARIS witness signatures.
- **DCA schedules** for sliced execution windows.

