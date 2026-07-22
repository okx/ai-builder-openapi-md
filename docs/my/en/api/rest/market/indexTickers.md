---
site: my
lang: en
name: Get index tickers
method: GET
path: /api/v5/market/index-tickers
module: market
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get index tickers

Retrieve index tickers.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/index-tickers`

> Request Example

```shell
GET /api/v5/market/index-tickers?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve index tickers
result = marketDataAPI.get_index_tickers(
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter   | Type   | Required | Description                                                                                          |
| :------- | :----- | :------- |:-----------------------------------------------------------------------------------------------------|
| quoteCcy | String | Conditional      | Quote currency <br>Currently there is only an index with `USD/USDT/BTC/USDC` as the quote currency. |
| instId   | String | Conditional      | Index, e.g. `BTC-USD`<br>Either `quoteCcy` or `instId` is required. <br>Same as `uly`.                                 |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT",
            "idxPx": "43350",
            "high24h": "43649.7",
            "sodUtc0": "43444.1",
            "open24h": "43640.8",
            "low24h": "43261.9",
            "sodUtc8": "43328.7",
            "ts": "1649419644492"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                    |
| :--------- | :------- | :---------------------------------------------------------- |
| instId     | String   | Index                                                   |
| idxPx      | String   | Latest index price                               |
| high24h    | String   | Highest price in the past 24 hours |
| low24h     | String   | Lowest price in the past 24 hours   |
| open24h    | String   | Open price in the past 24 hours     |
| sodUtc0    | String   | Open price in the UTC 0   |
| sodUtc8    | String   | Open price in the UTC 8 |
| ts         | String   | Index price update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
