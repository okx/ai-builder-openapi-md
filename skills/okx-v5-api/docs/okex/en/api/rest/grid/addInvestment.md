---
site: okex
lang: en
name: POST / Add investment
method: POST
path: /api/v5/tradingBot/grid/adjust-investment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Add investment

It is used to add investment and only applicable to contract gird.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/adjust-investment`

> Request Example

```shell
POST /api/v5/tradingBot/grid/adjust-investment
body
{
    "algoId":"448965992920907776",
    "amt":"12"
}
```

#### Request Parameters

| Parameter | Type    | Required   | Description                     |
|:----------| :------ |:-----------|:--------------------------------|
| algoId    | String | Yes        | Algo ID                         |
| amt    | String | Yes        | The amount is going to be added |
| allowReinvestProfit    | String | No       | Whether reinvesting profits, only applicable to spot grid.<br>`true` or `false`. The default is true.                         |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "448965992920907776"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                   |
|:--------------| :------- |:----------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                           |
