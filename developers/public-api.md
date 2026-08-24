# Public REST API

The Public REST API exposes read-only market and account data for dashboards, widgets, and lightweight integrations. It is **free and unauthenticated**, but **rate-limited** to keep the service reliable for everyone.

For programmatic trading, position management, and higher throughput, use the [Trading API](./trading-api.md) instead.

## Base URL

```text
https://addax.finance
```

## Rate limits

| Scope | Limit |
|---|---|
| Public REST (per IP) | **60 requests / minute** |

Every response includes:

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1734567890
```

When exceeded, the API returns `429`:

```json
{
  "error": "rate_limit_exceeded",
  "message": "Public API rate limit exceeded. Use the Trading API for higher limits.",
  "retryAfterSec": 34
}
```

## Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/perp/market-stats` | Per-market stats and reference marks |
| `GET` | `/api/perp/vault-mark` | On-chain DIA mark (`?symbol=BTC` or `?diaKey=BTC/USD`) |
| `GET` | `/api/perp/positions` | Open positions (`?account=0x…`) |
| `GET` | `/api/perp/open-limits` | Pending limit orders (`?account=0x…`) |
| `GET` | `/api/perp/trades` | Trade history (`?account=0x…`, optional `symbol`, `limit`) |
| `GET` | `/api/perp/candles` | OHLCV candles (`?asset=bitcoin&interval=15m`) |
| `GET` | `/api/perp/prices` | Reference spot quotes (CoinGecko proxy) |

### Examples

```bash
# Mark price
curl -s "https://addax.finance/api/perp/vault-mark?symbol=BTC"

# Open positions
curl -s "https://addax.finance/api/perp/positions?account=0xYourAddress"

# Market stats
curl -s "https://addax.finance/api/perp/market-stats"
```

## When to use the Trading API instead

| Need | Public API | Trading API |
|---|---|---|
| Display prices / charts | Yes | Optional |
| Read positions occasionally | Yes | Yes (higher limits) |
| Open / close / update positions | No | **Yes** |
| Prepared unsigned transactions | No | **Yes** |
| Production bots / apps | Limited | **Recommended** |

See [Trading API](./trading-api.md) for `POST /api/v1/orders/prepare` and authenticated reads at `/api/v1/positions`.

## Related

- [Fetching Prices](./fetching-prices.md) — DIA oracle keys and direct on-chain reads
- [Integration Overview](./README.md)
