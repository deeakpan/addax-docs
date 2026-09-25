# What is Addax

Addax matches orders on a central limit order book.

A limit order that rests on the book makes liquidity. An order that trades against it takes liquidity. The price is the book price. A market order fills against the current bids or asks, down to the worst price you allow. What cannot be filled at that price is canceled.

You can also place a stop, a take profit, or a TWAP. A stop or take profit waits for its trigger, then enters the book as a market or limit order. A TWAP splits the order into smaller fills over time.

Positions pay funding once an hour. If the rate is positive, longs pay shorts. If it is negative, shorts pay longs.

Login is through [Para](https://www.getpara.com/). You can connect MetaMask or use a social login. Either way you get an embedded wallet, and that wallet is yours on mainnet and on testnet.

ADDX is a collateral asset. So is zkLTC. Addax routes both into USDC on Ethereum, and that USDC is the margin for the order book position. People who stake ADDX receive 40% of protocol fees.

Addax is built on [Lighter](https://lighter.xyz/).
