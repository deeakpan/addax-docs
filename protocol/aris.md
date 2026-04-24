# ARIS and Intents

ARIS orders are signed offchain intents that authorize constrained execution onchain.

## Intent model

- User signs a witness payload (EIP-712 style).
- Signature includes token, amount, nonce, deadline, and reactor-specific fields.
- A filler executes only if constraints are satisfied.

## Security controls

- Permit2 nonce-based replay protection.
- Deadline bounds.
- Reactor-side validation logic.
- Optional additional validation fields.

## Why intents

- Flexible order types (limit, DCA, dutch variants).
- Offchain UX, onchain settlement guarantees.
- Better composability for filler networks.

