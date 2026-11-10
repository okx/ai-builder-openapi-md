---
site: okex
lang: en
name: POST / Add investment
method: POST
path: /api/v5/tradingBot/recurring/add-investment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Add investment
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/recurring/add-investment`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/add-investment
body
{
    "algoId": "2837428373700509696",
    "amount": "20"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| algoId | String | Yes | Algo order ID |
| amount | String | Yes | Additional investment amount. Only the investment currency used when the strategy was created is supported |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2837428373700509696",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### Response Parameters
| Parameter | Type | Description |
| :--- | :--- | :--- |
| algoId | String | Algo order ID |
| sCode | String | Event execution status code, `0` indicates success |
| sMsg | String | Error message if the event execution failed |
