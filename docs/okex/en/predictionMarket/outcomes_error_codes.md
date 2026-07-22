---
site: okex
lang: en
name: Error Codes
type: guide
---

# Error Codes

## 1. Response Format

All errors are returned in the following unified JSON structure:

```json
{
  "code": 100015,
  "msg": "Invalid calldata or malformed fields"
}
```

### 1.1 Common

May be returned by any OpenAPI v5 endpoint. For the full list of common error codes, see: [OKX public error codes](https://www.okx.com/docs-v5/en/#error-code-rest-api-public)

| Code | Meaning |
| --- | --- |
| 10000 | User not logged in |
| 10001 | Parameter validation failed |
| 10002 | Authentication failed |
| 206004 | Request oddsType does not match the account |

### 1.2 Event / Market

| Code | Meaning |
| --- | --- |
| 201001 | Event does not exist |
| 201002 | Market does not exist |

Applicable endpoints:

- GET /api/v5/predictions/events

- GET /api/v5/predictions/events/{eventId}

- GET /api/v5/predictions/events/{eventId}/markets

- GET /api/v5/predictions/markets/{marketId}

- GET /api/v5/predictions/events/search

## 2. Order / Write Operations

Covers: place order, cancel order, cancel all, heartbeat, Split, Merge, Redeem.

| Code | Meaning |
| --- | --- |
| 100001 | Market is not tradable |
| 100002 | Insufficient balance |
| 100006 | Account is frozen |
| 100010 | Insufficient token balance |
| 100011 | Order does not exist |
| 100012 | Order status does not allow cancellation |
| 100013 | Request address does not match the user address |
| 100015 | Invalid calldata or malformed fields |
| 100016 | Nonce already used |
| 100017 | Nonce expired |
| 100018 | Order amount is below the minimum notional |
| 100101 | TradeZone SDK signature exception |
| 120007 | User does not exist |
| 120022 | Account is in escape (exit) process |
| 201002 | Market does not exist |
| 213003 | Signature verified but TradeZone submission failed |

Applicable endpoints:

- `POST /api/v5/predictions/orders` Place order

- `POST /api/v5/predictions/orders/cancel` Cancel a single order

- `POST /api/v5/predictions/orders/cancel-all` Cancel all / market orders

- `POST /api/v5/predictions/heartbeat` Heartbeat

- `POST /api/v5/predictions/positions/split` Split

- `POST /api/v5/predictions/positions/merge` Merge

- `POST /api/v5/predictions/positions/redeem` Redeem

## 3. Order / Position Queries

Covers: query single order, order list, trade history, position queries.

| Code | Meaning |
| --- | --- |
| 100011 | Order does not exist |
| 400 | Path parameter parsing failed |

Applicable endpoints:

- `GET /api/v5/predictions/orders/{orderId}` Query a single order

- `GET /api/v5/predictions/orders` Query order list

- `GET /api/v5/predictions/trades` Query trade history

- `GET /api/v5/predictions/positions` Query positions
