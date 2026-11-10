---
site: okex
lang: en
name: POST / Stop recurring buy order
method: POST
path: /api/v5/tradingBot/recurring/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Stop recurring buy order

A maximum of 10 orders can be stopped per request.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID 

#### HTTP Request

`POST /api/v5/tradingBot/recurring/stop-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/stop-order-algo
body
[
    {
        "algoId":"560472804207104000"
    }
]
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                               |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1839309556514557952",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| algoId      | String   | Algo ID                                                                                        |
| algoClOrdId      | String   | Client-supplied Algo ID                                                                        |
| sCode      | String   | The code of the event execution result, 0 means success                                        |
| sMsg       | String   | Rejection message if the request is unsuccessful                                               |
| tag           | String   | <del>Order tag  </del>(Deprecated)                                                                                                                                                                                                                                                                  |
