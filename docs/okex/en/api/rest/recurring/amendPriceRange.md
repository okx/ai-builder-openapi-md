---
site: okex
lang: en
name: POST / Amend price range
method: POST
path: /api/v5/tradingBot/recurring/amend-price-range
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend price range
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/recurring/amend-price-range`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/amend-price-range
body
{
    "algoId": "2837428373700509696",
    "recurringList": [
        {
            "ccy": "BTC",
            "minPx": "80000",
            "maxPx": "120000"
        }
    ]
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| algoId | String | Yes | Algo order ID |
| recurringList | Array | Yes | Price range settings. The currency must be within the scope of the recurring buy currencies |
| >ccy | String | Yes | Recurring buy currency |
| >minPx | String | Yes | Minimum price of price range. `""` means no limit |
| >maxPx | String | Yes | Maximum price of price range. `""` means no limit |

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
