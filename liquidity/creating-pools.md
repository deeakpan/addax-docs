# Creating Pools

Use the Create Liquidity page to deploy a new Addax V3 pool and optionally seed it with first liquidity.

## What you set

- Token A and Token B
- Fee tier
- Start price (`tokenB per 1 tokenA`)
- Optional initial amounts for first liquidity

## Behavior

- If both initial amounts are empty, the app creates an empty pool only.
- If both amounts are filled, the app creates the pool and submits initial liquidity in a second step.
- If a pool already exists for the selected pair and fee tier, creation is blocked.

## Tips

- Use realistic initial amounts relative to your start price; mismatched amounts can produce zero mintable liquidity.
- Start with a wider range for first liquidity if you are unsure about short-term volatility.
- Keep enough native gas token for both pool creation and initial mint transactions.
