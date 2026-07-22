---
site: okex
lang: en
name: POST / Close position
method: POST
path: /api/v5/tradingBot/signal/close-position
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Close position

Close the position of an instrument via a market order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/close-position`

> Request Example

```shell
POST /api/v5/tradingBot/signal/close-position
body
{
    "instId":"BTC-USDT-SWAP",
    "algoId":"448965992920907776"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId  | String | Yes   | Algo ID |
| instId  | String | Yes   | Instrument ID    |

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

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId     | String   | Algo ID |
