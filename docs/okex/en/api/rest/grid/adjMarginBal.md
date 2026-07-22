---
site: okex
lang: en
name: POST / Adjust margin balance
method: POST
path: /api/v5/tradingBot/grid/margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Adjust margin balance

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/margin-balance`

> Request Example

```shell
POST /api/v5/tradingBot/grid/margin-balance
body {
   "algoId":"123456",
   "type":"add",
   "amt":"10"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                                   |
| type    | String | Yes       | Adjust margin balance type<br>`add` `reduce`                                                  |
| amt    | String | Conditional       | Adjust margin balance amount<br>Either `amt` or `percent` is required.                                              |
| percent    | String | Conditional       | Adjust margin balance percentage                                              |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "123456"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId        | String | Algo ID                                                   |
| algoClOrdId   | String   | Client-supplied Algo ID                                                    |
