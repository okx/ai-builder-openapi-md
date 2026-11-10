---
site: okex
lang: en
name: POST / Amend recurring buy order
method: POST
path: /api/v5/tradingBot/recurring/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Amend recurring buy order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID 

#### HTTP Request

`POST /api/v5/tradingBot/recurring/amend-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/amend-order-algo
body
{
    "algoId":"448965992920907776",
    "stgyName":"stg1"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                  |
| :--------- | :------ | :------- |:-----------------------------------------------------------------------------|
| algoId    | String | Yes       | Algo ID                                                                      |
| stgyName    | String | Yes       | New custom name for trading bot after adjustment, no more than 40 characters |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"448965992920907776",
            "algoClOrdId":"",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| algoId      | String   | Algo ID                                                                                        |
| algoClOrdId      | String   | Client-supplied Algo ID                                                                        |
| sCode      | String   | The code of the event execution result, 0 means success                                        |
| sMsg       | String   | Rejection message if the request is unsuccessful                                               |
