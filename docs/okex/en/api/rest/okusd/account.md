---
site: okex
lang: en
name: GET / Get OKUSD Account
method: GET
path: /api/v5/finance/okusd/account
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get OKUSD Account

Retrieve your current OKUSD balance and lifetime accrued yield. All balances are aggregated at the master-account level and shared across sub-accounts.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/okusd/account`

> Request Example

```shell
GET /api/v5/finance/okusd/account
```

#### Request Parameters

None

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ccy": "OKUSD",
            "amt": "10000.00000000",
            "totalEarnAccrual": "123.45678900",
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| ccy | String | Currency. Always `"OKUSD"` |
| amt | String | Current OKUSD balance |
| totalEarnAccrual | String | Cumulative yield accrued over the holding period, denominated in USDT |
| ts | String | Server timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
