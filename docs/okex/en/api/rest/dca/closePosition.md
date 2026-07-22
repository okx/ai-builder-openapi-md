---
site: okex
lang: en
name: POST / Stop dca algo order
method: POST
path: /api/v5/tradingBot/dca/stop
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Stop dca algo order
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/dca/stop`

> Request Example

```shell
POST /api/v5/tradingBot/dca/stop
body
{
    "algoOrdType": "contract_dca",
    "algoId": "448965992920907776",
    "stopType": "1"
}
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                                                                                                                      |
| :---------- | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId      | String | Yes      | Algo ID                                                                                                                                                          |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order                                                                              |
| stopType    | String | Yes      | Stop type<br>`contract_dca`: `1`: Market close all positions, `2`: Keep positions<br>`spot_dca`: `1`: Sell base currency, `2`: Keep base currency                |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "448965992920907776",
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
| tag         | String | Order tag                                                     |
| algoClOrdId | String | Client-supplied Algo ID                                       |
| sCode       | String | The code of the event execution result, 0 means success       |
| sMsg        | String | Rejection message if the request is unsuccessful              |
