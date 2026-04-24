# Order API

The Addax order API is the offchain relay between users and fillers. Users submit signed orders to it. Fillers poll it to find open orders to execute.

**Base URL:** `https://addax.finance/api`

---

## GET /api/orders

Fetch open orders. Fillers poll this to find work.

**Query parameters:**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `chainId` | number | `4441` | Chain ID |
| `orderStatus` | string | `open` | `open`, `filled`, `cancelled`, `expired`, or `all` |
| `orderType` | string | — | Filter by type: `Limit`, `Dutch_V2`, `DCA`, etc. |
| `limit` | number | `100` | Max results, capped at 1000 |

**Example:**

```bash
curl "https://addax.finance/api/orders?chainId=4441&orderStatus=open&orderType=Limit&limit=50"
```

**Response:**

```json
{
  "orders": [
    {
      "id": 1,
      "orderHash": "0xabc...",
      "chainId": 4441,
      "orderType": "Limit",
      "orderStatus": "open",
      "encodedOrder": "0x...",
      "signature": "0x...",
      "offerer": "0xuser...",
      "filler": null,
      "quoteId": null,
      "expiresAt": "2026-04-25T00:00:00.000Z",
      "createdAt": "2026-04-24T12:00:00.000Z",
      "updatedAt": "2026-04-24T12:00:00.000Z"
    }
  ]
}
```

---

## POST /api/orders

Submit a new signed order.

**Body:**

```json
{
  "orderHash": "0x...",
  "encodedOrder": "0x...",
  "signature": "0x...",
  "orderType": "Limit",
  "orderStatus": "open",
  "offerer": "0xuser...",
  "expiresAt": "2026-04-25T00:00:00.000Z",
  "chainId": 4441
}
```

**TypeScript:**

```typescript
const res = await fetch("https://addax.finance/api/orders", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    orderHash,
    encodedOrder,
    signature,
    orderType: "Limit",
    offerer: userAddress,
    expiresAt: new Date(deadline * 1000).toISOString(),
    chainId: 4441,
  }),
});
const { order } = await res.json();
```

**Python:**

```python
import requests

res = requests.post("https://addax.finance/api/orders", json={
    "orderHash": order_hash,
    "encodedOrder": encoded_order,
    "signature": signature,
    "orderType": "Limit",
    "offerer": user_address,
    "expiresAt": expires_at_iso,
    "chainId": 4441,
})
order = res.json()["order"]
```

---

## POST /api/orders/cancel

Cancel an open order. Updates its status to `cancelled` in the database. Note: this is an offchain cancellation. For guaranteed onchain cancellation, invalidate the Permit2 nonce directly.

**Body:**

```json
{
  "orderHash": "0x...",
  "chainId": 4441,
  "reason": "user_cancel"
}
```

---

## GET /api/dca

Same as `GET /api/orders` but pre-filtered to `orderType=DCA`.

## POST /api/dca

Same as `POST /api/orders` — submits a DCA slice order.

---

## Order status lifecycle

```
open → filled      (filler executed it onchain)
open → cancelled   (user cancelled via API or Permit2 nonce invalidation)
open → expired     (deadline passed without fill)
```
