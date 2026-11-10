---
site: turkey
lang: en
name: GET / 24H total volume
method: GET
path: /api/v5/market/platform-24-volume
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 24H total volume

The 24-hour trading volume is calculated on a rolling basis.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/platform-24-volume`

> Request Example

```shell
GET /api/v5/market/platform-24-volume

```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve 24 total volume
result = marketDataAPI.get_volume()
print(result)
```

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
         "volCny": "230900886396766",
         "volUsd": "34462818865189",
         "ts": "1657856040389"
     }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**       |
| :--------- | :------- | :----------------------------------------------------------- |
| volUsd     | String     | 24-hour total trading volume from the order book trading in "USD"              |
| volCny     | String     | 24-hour total trading volume from the order book trading in "CNY" |
| ts         | String     | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085`                              |
