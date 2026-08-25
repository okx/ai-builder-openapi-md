---
site: okex
lang: en
name: POST / Amend dca reinvestment
method: POST
path: /api/v5/tradingBot/dca/settings/reinvestment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend dca reinvestment
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`POST /api/v5/tradingBot/dca/settings/reinvestment`

> Request Example

```shell
POST /api/v5/tradingBot/dca/settings/reinvestment
body
{
    "algoId": "2833925189933756416",
    "algoOrdType": "contract_dca",
    "allowReinvest": true
}
```

#### Request Parameters
| Parameter     | Type    | Required | Description                                         |
| :------------ | :------ | :------- | :-------------------------------------------------- |
| algoId        | String  | Yes      | Algo ID                                             |
| algoOrdType   | String  | Yes      | Algo order type<br>`contract_dca`: Contract DCA order |
| allowReinvest | Boolean | Yes      | Whether to reinvest profit<br>`true` or `false`     |

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
| Parameter   | Type   | Description                                                   |
| :---------- | :----- | :------------------------------------------------------------ |
| algoId      | String | Algo ID                                                       |
| algoOrdType | String | Algo order type<br>`contract_dca`: Contract DCA order         |
| sCode       | String | The code of the event execution result, 0 means success       |
| sMsg        | String | Rejection message if the request is unsuccessful              |
