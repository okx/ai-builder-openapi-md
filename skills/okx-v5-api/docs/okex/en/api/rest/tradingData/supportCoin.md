---
site: okex
lang: en
name: Get support coin
method: GET
path: /api/v5/rubik/stat/trading-data/support-coin
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

# Trading Statistics
## REST API
The API endpoints of `Trading Statistics` do not require authentication.
### Get support coin

Retrieve the currencies supported by the trading statistics endpoints.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/trading-data/support-coin`

> Request Example

```shell
GET /api/v5/rubik/stat/trading-data/support-coin
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the currencies supported by the trading statistics endpoints
result = tradingDataAPI.get_support_coin()
print(result)
```
> Response Example

```json
{
    "code": "0",
    "data": {
        "contract": [
            "ADA",
            "BTC"
        ],
        "option": [
            "BTC"
        ],
        "spot": [
            "ADA",
            "BTC"
        ]
    },
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type**                | **Description**                                            |
| :------------ |:------------------------| :--------------------------------------------------------- |
| contract     | Array of strings         | Currency supported by derivatives trading data                                               |
| option       | Array of strings         | Currency supported by option trading data                                |
| spot         | Array of strings         | Currency supported by spot trading data                                  |
