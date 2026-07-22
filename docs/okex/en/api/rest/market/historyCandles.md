---
site: okex
lang: en
name: GET / Candlesticks history
method: GET
path: /api/v5/market/history-candles
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Candlesticks history

Retrieve history candlestick charts from recent years(It is last 3 months supported for 1s candlestick).

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/history-candles`

> Request Example

```shell
GET /api/v5/market/history-candles?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve history candlestick charts from recent years
result = marketDataAPI.get_history_candlesticks(
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                   |
| :----- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId | String | Yes       | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                |
| after  | String | No       | Pagination of data to return records earlier than the requested `ts`                                                                                                                                                          |
| before | String | No       | Pagination of data to return records newer than the requested `ts`. The latest data will be returned when using `before` individually                                                                                         |
| bar    | String | No       | Bar size, the default is `1m`<br>e.g. [1s/1m/3m/5m/15m/30m/1H/2H/4H] <br>UTC+8 opening price k-line: [6H/12H/1D/2D/3D/1W/1M/3M]<br>UTC+0 opening price k-line: [6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/1Wutc/1Mutc/3Mutc] |
| limit  | String | No       | Number of results per request. The maximum is `300`. The default is `100`.                                                                                                                                                    |
| adjust | String | No       | Price adjustment type for equity perpetual contracts.<br>`forward`: Forward adjustment.<br>If this field is omitted, unadjusted data is returned by default.<br>Only applicable to equity perpetual contracts. |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
     [
        "1597026383085",
        "3.721",
        "3.743",
        "3.677",
        "3.708",
        "8422410",
        "22698348.04828491",
        "12698348.04828491",
        "1"
    ],
    [
        "1597026383085",
        "3.731",
        "3.799",
        "3.494",
        "3.72",
        "24912403",
        "67632347.24399722",
        "37632347.24399722",
        "1"
    ]
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                      |
|:--------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ts            | String   | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                         |
| o             | String   | Open price                                                                                                                                                                                           |
| h             | String   | Highest price                                                                                                                                                                                        |
| l             | String   | Lowest price                                                                                                                                                                                         |
| c             | String   | Close price                                                                                                                                                                                          |
| vol           | String   | Trading volume, with a unit of `contract`. <br>If it is a `derivatives` contract, the value is the number of contracts. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in base currency.      |
| volCcy        | String   | Trading volume, with a unit of `currency`. <br>If it is a `derivatives` contract, the value is the number of base currency. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in quote currency. |
| volCcyQuote   | String  | Trading volume, the value is the quantity in quote currency<br>e.g. The unit is USDT for BTC-USDT and BTC-USDT-SWAP;<br>The unit is USD for BTC-USD-SWAP                                             |
| confirm       | String  | The state of candlesticks<br>`0`: K line is uncompleted<br>`1`: K line is completed                                                                                                                  |

<aside class="notice">
 <p>
   The data returned will be arranged in an array like this: [ts,o,h,l,c,vol,volCcy,volCcyQuote,confirm]
 </p>
</aside>

<aside class="notice">
1s candle is not supported by OPTION, but it is supported by other business lines (SPOT, MARGIN, FUTURES and SWAP)
</aside>

<aside class="notice">
When <code>adjust=forward</code>, the historical OHLC prices are multiplied by the adjustment factor for the relevant period. For stock splits, <code>vol</code> and <code>volCcy</code> are also adjusted proportionally. <code>volCcyQuote</code> is not adjusted. This parameter is only effective for equity perpetual contracts.
</aside>
