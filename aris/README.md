# ARIS

ARIS is the intent execution layer of Addax. It lets users create orders offchain with no gas cost. A decentralised network of fillers monitors open orders and executes them onchain when conditions are met.

## Order types

| Type | Contract | Use case |
|---|---|---|
| Limit | ArisLimitOrderReactor | Fill at or better than a fixed price |
| Dutch | ArisDutchOrderReactor | Price decays from high to low until filled |
| Exclusive Dutch | ArisExclusiveDutchOrderReactor | Dutch with a priority filler window |
| DCA | ArisDcaOrderReactor | Recurring buys split across time intervals |

## Sections

- [How Intents Work](./intents.md)
- [Limit Orders](./limit-orders.md)
- [Dutch Orders](./dutch-orders.md)
- [DCA Orders](./dca.md)
- [Order API](./order-api.md)
- [Becoming a Filler](./becoming-a-filler.md)
