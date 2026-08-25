---
site: okex
lang: en
name: GET / APY history
method: GET
path: /api/v5/finance/stable-rewards/apy-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / APY history

Retrieve the historical daily APY of the specified stablecoin. The returned rate reflects the user's current VIP level.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/finance/stable-rewards/apy-history`

> Request Example

```shell
GET /api/v5/finance/stable-rewards/apy-history?ccy=USDG
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| ccy | String | Yes | Stablecoin, e.g. `USDG` |
| days | String | No | Number of historical days to return. The default is `100`. The maximum is `100` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "rate": "0.004",
            "ts": "1718035200000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| rate | String | Daily APY for the user's current VIP level, e.g. `0.041` represents `4.1%` |
| ts | String | Snapshot time (UTC+0), Unix timestamp format in milliseconds, e.g. `1597026383085` |
