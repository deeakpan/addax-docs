# Opening & Closing Trades

Log in with [Para](https://www.getpara.com/) first, using MetaMask or a social login, so your embedded wallet is ready. Mainnet and testnet share that step. Testnet chain setup is under [Starting on testnet](../testnet/start.md).

## Open

1. Select a market.
2. Choose long or short.
3. Set size. Leverage cannot pass the cap for that market.
4. Choose a market, limit, stop, take profit, or TWAP.
5. Submit.

A market order matches the book immediately. A limit order rests at your price.

## Close

Send an order on the other side, or use a stop or take profit. Those wait for the trigger price, then enter the book. Cancel a resting order before it fills. If margin falls through the maintenance level, the position is liquidated.

On mainnet, ADDX or zkLTC collateral is routed to USDC on Ethereum before the position is opened. The USDC is on your Para embedded wallet.

Addax is built on [Lighter](https://lighter.xyz/).
