---
site: okex
lang: en
name: GET / Get Rewards History
method: GET
path: /api/v5/finance/okusd/rewards/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get Rewards History

Retrieve your daily OKUSD yield distribution history. Results are returned in descending order by timestamp (newest first).

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### Permission: Read

#### HTTP Request

`GET /api/v5/finance/okusd/rewards/history`

> Request Example

```shell
GET /api/v5/finance/okusd/rewards/history?limit=7
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| limit | String | No | Number of results per page. Default `"30"`. Maximum `"100"` |
| begin | String | No | Start time filter (`ts`, Unix ms, inclusive). Maximum query span is 6 months |
| end | String | No | End time filter (`ts`, Unix ms, inclusive). Defaults to the current time if not provided |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ccy": "USDT",
            "earnAmt": "1.14246575",
            "amt": "10000.00000000",
            "apr": "0.0418",
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| ccy | String | Yield currency. Always `"USDT"` |
| earnAmt | String | USDT yield distributed in this event |
| amt | String | Your USDT principal balance at the time of distribution |
| apr | String | APR applied for this distribution, e.g. `"0.0418"` represents 4.18% |
| ts | String | Distribution timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
