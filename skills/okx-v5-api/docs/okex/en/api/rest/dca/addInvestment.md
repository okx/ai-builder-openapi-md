---
site: okex
lang: en
name: POST / Add investment
method: POST
path: /api/v5/tradingBot/dca/orders/manual-buy
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Add investment
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`POST /api/v5/tradingBot/dca/orders/manual-buy`

> Request Example

```shell
POST /api/v5/tradingBot/dca/orders/manual-buy
body
{
    "algoId": "2833925189933756416",
    "algoOrdType": "contract_dca",
    "price": "41000",
    "amt": "100"
}
```

#### Request Parameters
| Parameter      | Type   | Required | Description                                                                          |
| :------------- | :----- | :------- | :----------------------------------------------------------------------------------- |
| algoId         | String | Yes      | Algo ID                                                                              |
| algoOrdType    | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order  |
| price          | String | Yes      | Manual added order limit price                                                       |
| amt            | String | Yes      | Amount                                                                               |
| ordType        | String | No       | Order type<br>`limit`: Limit order<br>`market`: Market order<br>Only applicable to `spot_dca` |
| tradeQuoteCcy  | String | No       | Quote currency for trading<br>Only applicable to `spot_dca`                          |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoClOrdId": "",
            "algoOrdType": "contract_dca",
            "tag": "",
            "diffAmount": "100",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### Response Parameters
| Parameter   | Type   | Description                                                                          |
| :---------- | :----- | :----------------------------------------------------------------------------------- |
| algoId      | String | Algo ID                                                                              |
| algoClOrdId | String | Client-supplied Algo ID                                                              |
| algoOrdType | String | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order  |
| tag         | String | Order tag                                                                            |
| diffAmount  | String | Extra amount transferred from trading account<br>Only applicable to `contract_dca`   |
| sCode       | String | The code of the event execution result, 0 means success                              |
| sMsg        | String | Rejection message if the request is unsuccessful                                     |
