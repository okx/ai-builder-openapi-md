---
site: okex
lang: en
name: Get contract open interest history
method: GET
path: /api/v5/rubik/stat/contracts/open-interest-history
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get contract open interest history

Retrieve the contract open interest statistics of futures and perp. This endpoint can retrieve the latest 1,440 data entries. <br>

For period=1D, the data time range is up to January 1, 2024; for other periods, the data time range is up to early February 2024.

#### Rate limit: 10 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID
#### HTTP Request

`GET /api/v5/rubik/stat/contracts/open-interest-history`

> Request example

```shell
GET /api/v5/rubik/stat/contracts/open-interest-history?instId=BTC-USDT-SWAP
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the open interest history
result = tradingDataAPI.get_open_interest_history(
    instId="BTC-USDT-SWAP"
)

print(result)
```

#### Request parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| instId | string | Yes | Instrument ID, eg: BTC-USDT-SWAP <br>Only applicable to `FUTURES`, `SWAP` |
| period | string | No | Bar size, the default is `5m`, e.g. [`5m/15m/30m/1H/2H/4H`] <br>UTC+8 opening price k-line: [`6H/12H/1D/2D/3D/5D/1W/1M/3M`] <br>UTC+0 opening price k-line: [`6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/5Dutc/1Wutc/1Mutc/3Mutc`] |
| end | string | No | Pagination of data to return records earlier than the requested `ts` |
| begin | string | No | return records newer than the requested `ts` |
| limit | string | No | Number of results per request. The maximum is `100`. The default is `100`. |

> Response example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "1701417600000",    // timestamp
            "731377.57500501",   // open interest (oi, contracts)
            "111",              // open interest (oiCcy, coin)
            "8888888"         // open interest (oiUsd, USD)
        ],
        [
            "1701417500000",    // timestamp
            "731377.57500501",   // open interest (oi, contracts)
            "111",              // open interest (oiCcy, coin)
            "8888888"         // open interest (oiUsd, USD)
        ]
    ]
}

```

#### Response parameters

| Parameter | Type | Description |
| --- | --- | --- |
| ts | String | Timestamp, millisecond format of Unix timestamp, e.g. `1597026383085` |
| oi | String | Open interest in the unit of contracts |
| oiCcy | String | Open interest in the unit of crypto |
| oiUsd | String | Open interest in the unit of USD |

The data returned will be arranged in an array like this: [ts, oi, oiCcy, oiUsd].
