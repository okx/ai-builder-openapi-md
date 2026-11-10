---
site: okex
lang: en
name: POST / Cancel signal bots
method: POST
path: /api/v5/tradingBot/signal/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Cancel signal bots

A maximum of 10 orders can be stopped per request.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/stop-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/signal/stop-order-algo
body
[
    {
        "algoId":"448965992920907776"
    }
]
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId        | String   | Yes       | Algo ID                                                   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": "",
            "algoClOrdId": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId      | String   | Algo ID                          |
| sCode       | String   | The code of the event execution result, `0` means success.               |
| sMsg        | String   | Rejection or success message of event execution.                     |
| algoClOrdId      | String   | Client-supplied Algo ID                                                                        |
