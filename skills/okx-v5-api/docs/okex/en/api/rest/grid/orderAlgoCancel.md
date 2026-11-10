---
site: okex
lang: en
name: POST / Stop grid algo order
method: POST
path: /api/v5/tradingBot/grid/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Stop grid algo order

A maximum of 10 orders can be stopped per request.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/stop-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/grid/stop-order-algo
body
[
    {
        "algoId":"448965992920907776",
        "instId":"BTC-USDT",
        "stopType":"1",
        "algoOrdType":"grid"
    }
]
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                       |
| :--------- | :------ | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId    | String | Yes       | Algo ID                                                                                                                                           |
| instId    | String | Yes       | Instrument ID, e.g. `BTC-USDT`                                                                                                                    |
| algoOrdType | String | Yes       | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                            |
| stopType | String | Yes       | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions       |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                           |
|:--------------| :------- |:--------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                   |
| algoClOrdId   | String   | Client-supplied Algo ID                                                   |
| sCode         | String   | The code of the event execution result, `0` means success.                |
| sMsg          | String   | Rejection message if the request is unsuccessful.                         |
| tag           | String   | Order tag                                                  |
