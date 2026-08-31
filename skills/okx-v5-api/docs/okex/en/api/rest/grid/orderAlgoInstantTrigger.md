---
site: okex
lang: en
name: POST / Instant trigger grid algo order
method: POST
path: /api/v5/tradingBot/grid/order-instant-trigger
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Instant trigger grid algo order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/order-instant-trigger`

> Request Example

```shell
POST /api/v5/tradingBot/grid/order-instant-trigger
body
{
    "algoId":"561564133246894080"
}

```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                           |
| :--------- | :------ | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId    | String | Yes       | Algo ID                                                                                                                                               |
| topUpAmt   | String | No       | Top up amount, only applicable to spot grid                                                                          |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "561564133246894080"
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
