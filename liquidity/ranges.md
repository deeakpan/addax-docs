# Ranges

Ranges define where your liquidity is active.

## Tick and range intuition

- Price space is discretized into ticks.
- A position is active when current price tick is between `tickLower` and `tickUpper`.
- Fee tier implies tick spacing constraints.

## Range strategy patterns

- **Tight active range** for directional/mean-reversion assumptions.
- **Balanced wider range** for passive exposure.
- **Layered ranges** (multiple NFTs) for custom risk profile.

## Risk notes

- Out-of-range positions stop earning fees.
- Inventory composition shifts as price moves (impermanent loss dynamics).

