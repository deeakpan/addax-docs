# Dutch Orders

A dutch order starts with a high asking price and decays linearly to a minimum over a defined time window. The first filler that finds the price acceptable fills it. This mechanism creates a competitive auction that surfaces fair market price without requiring the user to know it in advance.

**Reactor:** `ArisDutchOrderReactor` — `0xA70c28ef82d1427Fa68F65960FbfFb1198349593`

**Exclusive Dutch Reactor:** `ArisExclusiveDutchOrderReactor` — `0x538048AD8cC03Fe083CC5414a705D14ac0561331`

## Standard vs Exclusive Dutch

A **standard dutch order** is open to any filler from the moment it starts decaying.

An **exclusive dutch order** designates a priority filler for the first N seconds. During that window only the cosigner can fill. After the window expires, any filler can fill it — at the then-current decayed price. This is used when a routing partner has pre-negotiated better execution.

## How decay works

The output amount decays linearly from `startAmount` to `endAmount` between `decayStartTime` and `decayEndTime`:

```
outputAtTime = startAmount - (startAmount - endAmount) * (now - decayStartTime) / (decayEndTime - decayStartTime)
```

The filler fills at whatever the current decayed amount is. They keep the spread between their actual execution cost and the decayed amount.

## Example

User wants to sell 1 wzkLTC:
- At `t=0`: filler must deliver at least 100 USDC
- At `t=30s`: filler must deliver at least 95 USDC
- At `t=60s`: filler must deliver at least 90 USDC (floor)

A filler watching prices sees that at `t=15s`, 1 wzkLTC is worth 98 USDC on Addax pools. The order requires 97.5 USDC at that moment. The filler fills it, earns 0.5 USDC spread.
