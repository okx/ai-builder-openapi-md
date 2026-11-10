---
site: okex
lang: en
name: Get historical market data
method: GET
path: /api/v5/public/market-data-history
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get historical market data

<aside class="notice">
<strong>Data availability</strong><br>
Historical data backfill is currently in progress. Data availability may vary by module, instrument, and time period. The dataset will be continuously expanded to provide more comprehensive historical coverage.
</aside>

<aside class="notice">
<strong>Legacy data format notice</strong><br>
For module 1 (trade history), some old historical files may contain column headers with both Chinese characters along with English column names. All the Chinese characters will be removed once the data backfill is done. Please account for this when parsing the data.
</aside>

<aside class="notice">
<strong>Data release schedule</strong><br>
Most data for modules 1, 2, 3, 11 is typically available on T+2; order book data is typically available on T+3.
</aside>

Retrieve historical market data for OKX.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/market-data-history`

> Request Example

```shell
GET /api/v5/public/market-data-history?module=1&instType=SWAP&instFamilyList=BTC-USDT&dateAggrType=daily&begin=1756604295000&end=1756777095000
```

#### Request Parameters

| Parameter      | Type           | Required | Description                                                                                                               |
|:---------------|:---------------|:---------|:--------------------------------------------------------------------------------------------------------------------------|
| module         | String         | Yes      | Data module type<br>`1`: Tick-by-tick trade history<br>`2`: 1-minute candlestick<br>`3`: Funding rate<br>`4`: 400-level orderbook<br>`5`: 5000-level orderbook (from Nov 1, 2025)<br>`6`: 50-level orderbook (will gradually be deprecated, please use module = `4`,`5` instead)<br>`11`: Borrowing rate |
| instType       | String         | Yes      | Instrument type<br>`SPOT`<br>`FUTURES`<br>`SWAP`<br>`OPTION` |
| instIdList     | String         | Conditional | List of instrument IDs, e.g. `BTC-USDT`, or `ANY` for all instruments (`ANY` is only supported for module = `1`, `2`, `3`, `11` & dateAggrType = `daily`)<br>Multiple instrument IDs should be separated by commas, e.g. `BTC-USDT,ETH-USDT`<br>Maximum length = 10<br>Only applicable when instType = `SPOT` |
| instFamilyList | String         | Conditional | List of instrument families, e.g. `BTC-USDT`, or `ANY` for all instruments (`ANY` is only supported for module = `1`, `2`, `3`, `11` & dateAggrType = `daily`)<br>Multiple instrument families should be separated by commas, e.g. `BTC-USDT,ETH-USDT`<br>Maximum length = 10 (= 1when module = `6` & instType = `OPTION`)<br>Only applicable when instType ≠ `SPOT` |
| dateAggrType   | String         | Yes      | Date aggregation type<br> `daily` (not supported for module = `3` & instFamilyList ≠ `ANY`)<br> `monthly` (not supported for module = `6`) |
| begin          | String         | Yes      | Begin timestamp. Unix timestamp format in milliseconds (inclusive)<br>Maximum range: 10 days for daily, 10 months for monthly |
| end            | String         | Yes      | End timestamp. Unix timestamp format in milliseconds (inclusive)<br>When module = `6` & instType = `OPTION`, only returns data for the day specified by `end` |

> Response Example

```json
{
  "code": "0",
  "data": [{
    "dateAggrType": "daily",
    "details": [{
      "dateRangeEnd": "1756656000000",
      "dateRangeStart": "1756569600000",
      "groupDetails": [{
        "dateTs": "1756656000000",
        "filename": "BTC-USDT-SWAP-trades-2025-09-01.zip",
        "sizeMB": "10.82",
        "url": "https://static.okx.com/cdn/okex/traderecords/trades/daily/20250901/BTC-USDT-SWAP-trades-2025-09-01.zip"
      },
      {
        "dateTs": "1756569600000",
        "filename": "BTC-USDT-SWAP-trades-2025-08-31.zip",
        "sizeMB": "4.82",
        "url": "https://static.okx.com/cdn/okex/traderecords/trades/daily/20250831/BTC-USDT-SWAP-trades-2025-08-31.zip"
      }],
      "groupSizeMB": "15.64",
      "instFamily": "BTC-USDT",
      "instId": "",
      "instType": "SWAP"
    }],
    "totalSizeMB": "15.64",
    "ts": "1756882260390"
  }],
  "msg": ""
}
```

> Response Example when no data files are available

```json
{
    "code": "0",
    "data": [
        {
            "dateAggrType": "monthly",
            "details": [],
            "totalSizeMB": "0",
            "ts": "1756889595507"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**    | **Type** | **Description**                                                                                                           |
|:-----------------|:---------|:--------------------------------------------------------------------------------------------------------------------------|
| ts               | String   | Response timestamp, Unix timestamp format in milliseconds |
| totalSizeMB      | String   | Total size of all data files in MB |
| dateAggrType     | String   | Date aggregation type<br> `daily`<br> `monthly` |
| details          | Array    | |
| > instId         | String   | Instrument ID |
| > instFamily     | String   | Instrument family |
| > dateRangeStart | String   | Data range start date, Unix timestamp format in milliseconds (inclusive) |
| > dateRangeEnd   | String   | Data range end date, Unix timestamp format in milliseconds (inclusive) |
| > groupSizeMB    | String   | Data group size in MB |
| > groupDetails   | Array    | |
| >> filename      | String   | Data file name, e.g. `BTC-USDT-SWAP-trades-2025-05-15.zip` |
| >> dataTs        | String   | Data date timestamp, Unix timestamp format in milliseconds |
| >> sizeMB        | String   | File size in MB |
| >> url           | String   | Download URL |

<aside class="notice">
<strong>Data query rules</strong><br>
• Only the date portion (yyyy-mm-dd) of timestamps is used; time components are ignored<br>
• Both begin and end timestamps are inclusive<br>
• Data is returned in reverse chronological order (closer to end first)<br>
• If the query exceeds record limits, data closest to the end timestamp is returned<br>
• <strong>Exception:</strong> When module = 6 & instType = OPTION, only data for the day specified by the end is returned
</aside>

<aside class="notice">
<strong>Timezone specifications for timestamp parsing</strong><br>
When converting Unix timestamps to dates, the following timezone conventions are applied to all timestamp fields (begin, end, dateRangeStart, dateRangeEnd, dataTs):<br>
• <strong>Orderbook data</strong> (modules 4, 5, 6): UTC+0<br>
• <strong>All other data modules</strong> (modules 1, 2, 3, 11): UTC+8
</aside>
