---
site: turkey
lang: en
name: Get exchange rate
method: GET
path: /api/v5/market/exchange-rate
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get exchange rate

This interface provides the average exchange rate data for 2 weeks

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/exchange-rate`

> Request Example

```shell
GET /api/v5/market/exchange-rate
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve average exchange rate data for 2 weeks
result = marketDataAPI.get_exchange_rate()
print(result)
```

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "usdCny": "7.162"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**       |
| :--------- | :------- | :----------------------------------------------------------- |
| usdCny     | String     | Exchange rate                                                   |
