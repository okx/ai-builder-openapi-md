---
site: okex
lang: en
name: POST / Reduce dca margin
method: POST
path: /api/v5/tradingBot/dca/margin/reduce
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Reduce dca margin
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`POST /api/v5/tradingBot/dca/margin/reduce`

> Request Example

```shell
POST /api/v5/tradingBot/dca/margin/reduce
body
{
    "algoId": "2833925189933756416",
    "amt": "50"
}
```

#### Request Parameters
| Parameter | Type   | Required | Description           |
| :-------- | :----- | :------- | :-------------------- |
| algoId    | String | Yes      | Algo order ID         |
| amt       | String | Yes      | Margin reduction amount |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoOrdType": "contract_dca",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### Response Parameters
| Parameter   | Type   | Description                                                              |
| :---------- | :----- | :----------------------------------------------------------------------- |
| algoId      | String | Algo order ID                                                            |
| algoOrdType | String | Algo order type<br>`contract_dca`: Contract DCA order                    |
| sCode       | String | Event execution status code, `0` indicates success                       |
| sMsg        | String | Error message if the event execution failed                              |
