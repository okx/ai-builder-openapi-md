---
site: okex
lang: en
name: GET / Balance
method: GET
path: /api/v5/finance/stable-rewards/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Balance

Retrieve the real-time Stable Rewards balance across the account (trading account, funding account, and in-progress redemptions combined), along with lifetime earnings and the current earning state for each stablecoin.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/stable-rewards/balance`

> Request Example

```shell
GET /api/v5/finance/stable-rewards/balance?ccy=USDG
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| ccy | String | No | Stablecoin, e.g. `USDG`<br>Returns all supported stablecoins if not specified |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "details": [
                {
                    "ccy": "USDG",
                    "amt": "100",
                    "totalEarnAccrual": "0.0003",
                    "state": "earning"
                }
            ],
            "ts": "1718035200000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| details | Array of objects | Real-time balance details per stablecoin |
| > ccy | String | Stablecoin, e.g. `USDG` |
| > amt | String | Currency amount held across the entire account |
| > totalEarnAccrual | String | Total interest accrued over the lifetime of the holding |
| > state | String | Earning state<br>`earning`: The balance is currently accruing rewards<br>`pending`: The balance is not currently accruing (e.g. auto-earn is off, or the balance is below the activation threshold) |
| ts | String | Query data time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
