# Limit Orders

Limit orders define minimum acceptable output for a specified input.

## How it works

- User signs a witness containing:
  - input token/amount
  - output token/min amount
  - nonce + deadline
  - reactor address
- Filler can execute when market conditions satisfy the min output.

## Cancellation

Recommended flow:

1. Invalidate nonce onchain first.
2. Confirm tx receipt.
3. Mark order cancelled in offchain storage/index.

This avoids "offchain-cancelled but still fillable onchain" states.

