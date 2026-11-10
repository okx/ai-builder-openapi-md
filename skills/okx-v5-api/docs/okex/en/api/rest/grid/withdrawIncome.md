---
site: okex
lang: en
name: POST / Spot grid withdraw income
method: POST
path: /api/v5/tradingBot/grid/withdraw-income
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Spot grid withdraw income

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/withdraw-income`

> Request Example

```shell
POST /api/v5/tradingBot/grid/withdraw-income
body
{
    "algoId":"448965992920907776"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                               |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId":"448965992920907776",
            "profit":"100"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type**   | **Description**                                            |
|:--------------|:-----------| :--------------------------------------------------------- |
| algoId        | String     | Algo ID                                                    |
| algoClOrdId   | String     | Client-supplied Algo ID                                                    |
| profit        | String     | Withdraw profit   |
