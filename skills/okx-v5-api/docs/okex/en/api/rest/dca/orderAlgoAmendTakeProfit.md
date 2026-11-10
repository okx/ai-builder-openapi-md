---
site: okex
lang: en
name: POST / Amend dca take profit settings
method: POST
path: /api/v5/tradingBot/dca/settings/take-profit
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Amend dca take profit settings
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`POST /api/v5/tradingBot/dca/settings/take-profit`

> Request Example

```shell
POST /api/v5/tradingBot/dca/settings/take-profit
body
{
    "algoId": "2833925189933756416",
    "algoOrdType": "contract_dca",
    "tpPrice": "43500"
}
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                    |
| :---------- | :----- | :------- | :------------------------------------------------------------- |
| algoId      | String | Yes      | Algo order ID                                                  |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order          |
| tpPrice     | String | Yes      | Take-profit price                                              |

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
| Parameter   | Type   | Description                                                        |
| :---------- | :----- | :----------------------------------------------------------------- |
| algoId      | String | Algo order ID                                                      |
| algoOrdType | String | Algo order type<br>`contract_dca`: Contract DCA order              |
| sCode       | String | Event execution status code, `0` indicates success                 |
| sMsg        | String | Error message if the event execution failed                        |
