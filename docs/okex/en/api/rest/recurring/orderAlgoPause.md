---
site: okex
lang: en
name: POST / Pause recurring buy
method: POST
path: /api/v5/tradingBot/recurring/pause
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Pause recurring buy
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/recurring/pause`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/pause
body
{
    "algoId": "2837428373700509696"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| algoId | String | Yes | Algo order ID |

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
