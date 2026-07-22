---
site: okex
lang: en
name: POST / Adjust margin balance
method: POST
path: /api/v5/tradingBot/signal/margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Adjust margin balance

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/margin-balance`

> Request Example

```shell
POST /api/v5/tradingBot/signal/margin-balance
body
{
   "algoId":"123456",
   "type":"add",
   "amt":"10"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                                   |
| type      | String | Yes       | Adjust margin balance type<br>`add` `reduce`  |
| amt       | String | Yes       | Adjust margin balance amount<br>Either `amt` or `percent` is required.  |
| allowReinvest | Boolean | No  | Whether to reinvest with newly added margin. The default value is `false`. <br>`false`:it will be used as passive margin to prevent liquidation and will not be used as active investment<br>`true`:the margin added here will furthermore be accounted for in calculations of your total investment amount, and furthermore your order size。<br>Only applicable to your signal comes in with an “investmentType” of “percentage_investment”  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "123456"
        }
    ],
    "msg": ""
}
```
#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId        | String   | Algo ID                        |
