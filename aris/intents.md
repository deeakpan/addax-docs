# How Intents Work

An ARIS intent is a signed message that describes a trade a user wants to make. The user signs it offchain. A filler reads it, decides whether to fill it, and submits the execution transaction to the reactor contract.

## Core data structures

Every order type shares a common `OrderInfo` header:

```solidity
struct OrderInfo {
    IReactor reactor;                       // which reactor handles this order
    address swapper;                        // the user who created the order
    uint256 nonce;                          // replay protection via Permit2
    uint256 deadline;                       // unix timestamp — order expires after this
    IValidationCallback additionalValidationContract; // optional extra validation
    bytes additionalValidationData;         // params passed to the validation contract
}
```

Tokens flow through two structs:

```solidity
struct InputToken {
    ERC20 token;       // token the user is selling
    uint256 amount;    // exact amount to transfer from user
    uint256 maxAmount; // upper bound (used for dutch decaying inputs)
}

struct OutputToken {
    address token;     // token the user wants to receive
    uint256 amount;    // minimum amount to deliver to recipient
    address recipient; // where to send the output
}
```

A filler submits a `SignedOrder`:

```solidity
struct SignedOrder {
    bytes order;  // ABI-encoded concrete order (LimitOrder, DcaOrder, etc.)
    bytes sig;    // EIP-712 signature from the swapper
}
```

## Execution flow

```
User                    Addax API               Filler              Reactor
 |                          |                      |                    |
 |-- sign order (EIP-712) ->|                      |                    |
 |-- POST /api/orders ----->|                      |                    |
 |                          |<-- GET /api/orders --|                    |
 |                          |-- open orders ------->|                    |
 |                          |                      |-- execute() ------>|
 |                          |                      |                    |-- validate sig
 |                          |                      |                    |-- check deadline
 |                          |                      |                    |-- transferFrom via Permit2
 |                          |                      |                    |-- transfer output to user
 |                          |                      |<-- Fill event -----|
```

## Security model

- **Permit2 nonces** — each order has a unique nonce. Once used it cannot be replayed.
- **Deadline** — the reactor reverts if `block.timestamp > order.deadline`.
- **Reactor binding** — the order encodes the reactor address. A signature for one reactor cannot be used on another.
- **Validation callbacks** — orders can specify an external contract to run custom pre-fill checks (price guards, allow-lists, etc.).
