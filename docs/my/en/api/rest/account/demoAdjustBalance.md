---
site: my
lang: en
name: Adjust demo account balance
method: POST
path: /api/v5/account/demo-adjust-balance
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Adjust demo account balance

**This endpoint is only applicable to the demo trading environment.**

Allows users to increase or reduce balances for specific currencies (BTC, ETH, USDT, OKB) in a demo account, enabling flexible testing of trading strategies under different capital scenarios.

All-or-nothing: if any currency in the request fails validation, the entire request is rejected and no balances are modified.

#### Rate Limit: Increase — 3 requests per user per day (resets at UTC 0:00). Reduce — no limit.
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/demo-adjust-balance`

> Request Example

```shell
POST /api/v5/account/demo-adjust-balance
body
{
    "type": "increase",
    "adjustments": [
        { "ccy": "BTC", "amt": "0.5" },
        { "ccy": "USDT", "amt": "3000" }
    ]
}
```

#### Request Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| type | String | Yes | Direction of adjustment.<br>`increase`: add to balance<br>`reduce`: deduct from balance<br>One direction per request; increase and reduce cannot be mixed. |
| adjustments | Array | Yes | List of currency adjustments. At least one item required. Duplicate currencies are not allowed. |
| > ccy | String | Yes | Currency. Supported values: `BTC` `ETH` `USDT` `OKB` |
| > amt | String | Yes | Adjustment amount. Must be non-negative. Decimal places must not exceed the precision defined for the currency.<br>Increase limits per request: BTC: 1, ETH: 1, USDT: 5000, OKB: 100.<br>Reduce has no per-request amount limit — only constrained by available balance ≥ 0. |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "remainCnt": "2",
        "totalCnt": "3",
        "details": [
            { "ccy": "BTC", "amt": "0.5", "bal": "1.5" },
            { "ccy": "USDT", "amt": "3000", "bal": "13000" }
        ]
    }]
}
```

> Failure Example

```json
{
    "code": "59693",
    "msg": "USDT transferable balance insufficient. Some funds are occupied by open orders or positions. Please cancel orders or close positions and try again",
    "data": []
}
```

#### Response Parameters

| Parameter | Type | Description |
| :--- | :--- | :--- |
| remainCnt | String | Remaining daily increase quota. Also returned for reduce requests, but reduce does not consume quota. |
| totalCnt | String | Total daily increase quota (default: 3). |
| details | Array | Per-currency operation details. |
| > ccy | String | Currency. |
| > amt | String | Adjustment amount applied. |
| > bal | String | Post-operation balance for this currency. |
