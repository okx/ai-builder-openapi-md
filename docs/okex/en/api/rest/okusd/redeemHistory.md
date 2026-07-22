---
site: okex
lang: en
name: GET / Get Redemption History
method: GET
path: /api/v5/finance/okusd/redeem/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get Redemption History

Retrieve your OKUSD redemption order history. Results are returned in descending order by timestamp (newest first) and support time-range filtering and redemption-type filtering.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### Permission: Read

#### HTTP Request

`GET /api/v5/finance/okusd/redeem/history`

> Request Example

```shell
GET /api/v5/finance/okusd/redeem/history?type=fast
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| limit | String | No | Number of results per page. Default `"100"`. Maximum `"100"` |
| begin | String | No | Start time filter (order creation time `ts`, Unix ms, inclusive) |
| end | String | No | End time filter (order creation time `ts`, Unix ms, inclusive) |
| type | String | No | Redemption type filter: `"fast"` for fast redemption only; `"standard"` for standard redemption only. Defaults to standard redemption if not specified |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678905678",
            "clOrdId": "my-rdm-fast-001",
            "ccy": "OKUSD",
            "amt": "1000.00000000",
            "fee": "1.00000000",
            "settleCcy": "USDT",
            "settleCcyAmt": "999.00000000",
            "type": "fast",
            "status": "success",
            "estSettlementTime": "1718500010000",
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
| ccy | String | Redemption currency. Always `"OKUSD"` |
| amt | String | OKUSD amount redeemed |
| fee | String | Fee charged in USDT, truncated (floor) to 8 decimal places |
| settleCcy | String | Settlement currency. Always `"USDT"` |
| settleCcyAmt | String | Net USDT amount credited (`amt - fee`, truncated to 8 decimal places) |
| type | String | Redemption type: `"fast"` (real-time settlement) or `"standard"` (D+5/D+6 calendar days) |
| status | String | Order status: `"pending"` / `"success"` / `"failed"` / `"canceled"` |
| estSettlementTime | String | Estimated settlement time, Unix timestamp format in milliseconds. Empty string for settled fast redemption orders |
| ts | String | Order creation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
