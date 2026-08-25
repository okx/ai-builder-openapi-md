---
site: turkey
lang: en
name: GET / Tickers
method: GET
path: /api/v5/market/tickers
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Tickers

Retrieve the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours. Best ask price may be lower than the best bid price during the pre-open period.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/tickers`

> Request Example

```shell
GET /api/v5/market/tickers?instType=SPOT

```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours
result = marketDataAPI.get_tickers(
    instType="SPOT"
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- |:---------|:--------------------------------------------------------------------|
| instType   | String | Yes      | Instrument type<br>`SPOT`  |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "instType": "SPOT",
            "instId": "BTC-USDT",
            "last": "51230",
            "lastSz": "0.18531491",
            "askPx": "51229.4",
            "askSz": "2.1683067",
            "bidPx": "51229.3",
            "bidSz": "0.28249897",
            "open24h": "51635.7",
            "high24h": "52080",
            "low24h": "50936",
            "volCcy24h": "539658490.410419122",
            "vol24h": "10476.2229261",
            "ts": "1708669508505",
            "sodUtc0": "51290.1",
            "sodUtc8": "51602.4"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                          |
| :--------- | :------- |:-----------------------------------------------------------------------------------------|
| instType   | String   | Instrument type                                                                          |
| instId     | String   | Instrument ID                                                                            |
| last       | String   | Last traded price                                                                        |
| lastSz     | String   | Last traded size                                                                         |
| askPx      | String   | Best ask price                                                                           |
| askSz      | String   | Best ask size                                                                            |
| bidPx      | String   | Best bid price                                                                           |
| bidSz      | String    | Best bid size                                                                            |
| open24h    | String   | Open price in the past 24 hours                                                          |
| high24h    | String   | Highest price in the past 24 hours                                                       |
| low24h     | String   | Lowest price in the past 24 hours                                                        |
| volCcy24h   | String   | 24h trading volume<br>If it is `SPOT`, the value is the quantity in quote currency.      |
| vol24h      | String   | 24h trading volume<br>If it is `SPOT`, the value is the quantity in base currency.       |
| sodUtc0    | String   | Open price in the UTC 0                                                                  |
| sodUtc8    | String   | Open price in the UTC 8                                                                  |
| ts         | String   | Ticker data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
