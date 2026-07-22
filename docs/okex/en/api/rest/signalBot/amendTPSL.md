---
site: okex
lang: en
name: POST / Amend TPSL
method: POST
path: /api/v5/tradingBot/signal/amendTPSL
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend TPSL

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/amendTPSL`

> Request Example

```shell
POST /api/v5/tradingBot/signal/amendTPSL
body
{
    "algoId": "637039348240277504",
    "exitSettingParam": {
        "tpSlType": "pnl",
        "tpPct": "0.01",
        "slPct": "0.01"
    }
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId                | String | Yes       | Algo ID  |
| exitSettingParam      | String | Yes       | Exit setting |
| > tpSlType            | String | Yes       | Type of set the take-profit and stop-loss trigger price<br>`pnl`: Based on the estimated profit and loss percentage from the entry point<br>`price`: Based on price increase or decrease from the crypto’s entry price |
| > tpPct               | String | No        | Take-profit percentage  |
| > slPct               | String | No        | Stop-loss percentage  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "637039348240277504"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId        | String   | Algo ID                        |
