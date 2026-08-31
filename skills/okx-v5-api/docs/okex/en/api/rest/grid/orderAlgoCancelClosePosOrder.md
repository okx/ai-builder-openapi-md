---
site: okex
lang: en
name: POST / Cancel close position order for contract grid
method: POST
path: /api/v5/tradingBot/grid/cancel-close-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Cancel close position order for contract grid

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/cancel-close-order`

> Request Example

```shell
POST /api/v5/tradingBot/grid/cancel-close-order
body
{
    "algoId":"448965992920907776",
    "ordId":"570627699870375936"
}
```

#### Request Parameters

| Parameter | Type      | Required   | Description             |
|:----------|:----------|:-----------|:------------------------|
| algoId    | String    | Yes        | Algo ID                 |
| ordId     | String   | Yes        | Close position order ID |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "ordId": "570627699870375936",
            "tag": ""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                   |
|:--------------| :------- |:----------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                           |
| ordId         | String   | Close position order ID |
| algoClOrdId        | String   | Client-supplied Algo ID                                                    |
| tag           | String   | Order tag                                                  |
