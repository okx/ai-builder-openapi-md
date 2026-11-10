---
site: us
lang: en
name: GET / Tickers
method: GET
path: /api/v5/market/tickers
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com
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
GET /api/v5/market/tickers?instType=SWAP

```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve the latest price snapshot, best bid/ask price, and trading volume in the last 24 hours
result = marketDataAPI.get_tickers(
    instType="SWAP"
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- |:---------|:--------------------------------------------------------------------|
| instType   | String | Yes      | Instrument type<br>`SPOT`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instFamily | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`            |                                                                                                                                                                      

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
        "instType":"SWAP",
        "instId":"LTC-USD-SWAP",
        "last":"9999.99",
        "lastSz":"1",
        "askPx":"9999.99",
        "askSz":"11",
        "bidPx":"8888.88",
        "bidSz":"5",
        "open24h":"9000",
        "high24h":"10000",
        "low24h":"8888.88",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "sodUtc0":"0.1",
        "sodUtc8":"0.1",
        "ts":"1597026383085"
     },
     {
        "instType":"SWAP",
        "instId":"BTC-USD-SWAP",
        "last":"9999.99",
        "lastSz":"1",
        "askPx":"9999.99",
        "askSz":"11",
        "bidPx":"8888.88",
        "bidSz":"5",
        "open24h":"9000",
        "high24h":"10000",
        "low24h":"8888.88",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "sodUtc0":"0.1",
        "sodUtc8":"0.1",
        "ts":"1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                            |
| :--------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType   | String   | Instrument type                                                                                                                                                                                            |
| instId     | String   | Instrument ID                                                                                                                                                                                              |
| last       | String   | Last traded price                                                                                                                                                                                          |
| lastSz     | String   | Last traded size. 0 represents there is no trading volume                                                                                                                                                                                           |
| askPx      | String   | Best ask price                                                                                                                                                                                             |
| askSz      | String   | Best ask size                                                                                                                                                                                              |
| bidPx      | String   | Best bid price                                                                                                                                                                                             |
| bidSz      | String    | Best bid size                                                                                                                                                                                              |
| open24h    | String   | Open price in the past 24 hours                                                                                                                                                                            |
| high24h    | String   | Highest price in the past 24 hours                                                                                                                                                                         |
| low24h     | String   | Lowest price in the past 24 hours                                                                                                                                                                          |
| volCcy24h   | String   | 24h trading volume, with a unit of `currency`. <br>If it is a `derivatives` contract, the value is the number of base currency. e.g. the unit is BTC for BTC-USD-SWAP and BTC-USDT-SWAP  <br>If it is `SPOT`/`MARGIN`, the value is the quantity in quote currency. |
| vol24h      | String   | 24h trading volume, with a unit of `contract`. <br>If it is a `derivatives` contract, the value is the number of contracts. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in base currency.      |
| sodUtc0    | String   | Open price in the UTC 0                                                                                                                                                                                    |
| sodUtc8    | String   | Open price in the UTC 8                                                                                                                                                                                    |
| ts         | String   | Ticker data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |
