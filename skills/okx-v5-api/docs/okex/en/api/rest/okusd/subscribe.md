---
site: okex
lang: en
name: POST / Subscribe OKUSD
method: POST
path: /api/v5/finance/okusd/subscribe
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Subscribe OKUSD

Subscribe USDT to receive OKUSD at a 1:1 rate with no subscription fee. OKUSD is credited immediately to your funding account. Pass a unique `clOrdId` per request; resubmitting the same `clOrdId` returns the original order without re-executing the subscription.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/okusd/subscribe`

> Request Example

```shell
POST /api/v5/finance/okusd/subscribe
body
{
    "amt": "1000.00000000",
    "clOrdId": "my-sub-001"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| amt | String | Yes | Amount of USDT to subscribe. Minimum: `1`. Maximum 8 decimal places. Scientific notation is not supported |
| clOrdId | String | Yes | Client-defined order ID. Maximum 32 characters (letters, digits, `-`, `_`). Must be unique per UID. Used for order tracking and idempotency |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678901234",
            "clOrdId": "my-sub-001",
            "ccy": "USDT",
            "amt": "1000.00000000",
            "okusdAmt": "1000.00000000",
            "state": "success",
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| ordId | String | System-generated order ID |
| clOrdId | String | Client-defined order ID (echoed back) |
| ccy | String | Subscription currency. Always `"USDT"` |
| amt | String | Actual USDT amount subscribed |
| okusdAmt | String | OKUSD amount credited to your funding account (equal to `amt` at 1:1 rate; no subscription fee) |
| state | String | Order status: `"success"` / `"pending"` / `"failed"` |
| ts | String | Order creation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
