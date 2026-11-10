---
site: okex
lang: en
name: GET / Get Subscription History
method: GET
path: /api/v5/finance/okusd/subscribe/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Get Subscription History

Retrieve your OKUSD subscription order history. Results are returned in descending order by timestamp (newest first) and support time-range filtering.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/okusd/subscribe/history`

> Request Example

```shell
GET /api/v5/finance/okusd/subscribe/history?limit=2
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| limit | String | No | Number of results per page. Default `"100"`. Maximum `"100"` |
| begin | String | No | Start time filter (order creation time `ts`, Unix ms, inclusive) |
| end | String | No | End time filter (order creation time `ts`, Unix ms, inclusive) |

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
            "settleCcy": "OKUSD",
            "settleCcyAmt": "1000.00000000",
            "status": "success",
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| ordId | String | System-generated order ID |
| clOrdId | String | Client-defined order ID (echoed back; empty string if not provided) |
| ccy | String | Subscription currency. Always `"USDT"` |
| amt | String | USDT amount subscribed |
| settleCcy | String | Settlement currency. Always `"OKUSD"` |
| settleCcyAmt | String | OKUSD amount credited (equal to `amt` at 1:1 rate) |
| status | String | Terminal order status: `"success"` / `"failed"` |
| ts | String | Order creation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
