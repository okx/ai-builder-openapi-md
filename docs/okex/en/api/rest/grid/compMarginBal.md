---
site: okex
lang: en
name: POST / Compute margin balance
method: POST
path: /api/v5/tradingBot/grid/compute-margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Compute margin balance

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/compute-margin-balance`

> Request Example

```shell
POST /api/v5/tradingBot/grid/compute-margin-balance
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
| amt    | String | No       | Adjust margin balance amount<br>Default is zero.                                              |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "lever": "0.3877200981166066",
            "maxAmt": "1.8309562403342999"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| maxAmt      | String   | Maximum adjustable margin balance amount                        |
| lever      | String   | Leverage after adjustment of margin balance                       |
