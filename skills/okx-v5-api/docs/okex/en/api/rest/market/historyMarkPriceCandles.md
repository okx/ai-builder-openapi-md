---
site: okex
lang: en
name: Get mark price candlesticks history
method: GET
path: /api/v5/market/history-mark-price-candles
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get mark price candlesticks history

Retrieve the candlestick charts of mark price from recent years.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/history-mark-price-candles`

> Request Example

```shell
GET /api/v5/market/history-mark-price-candles?instId=BTC-USD-SWAP
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USD-SWAP`                                 |
| after  | String | No       | Pagination of data to return records earlier than the requested `ts` |
| before | String | No       | Pagination of data to return records newer than the requested `ts`. The latest data will be returned when using `before` individually |
| bar    | String | No       | Bar size, the default is `1m`<br>e.g. [1m/3m/5m/15m/30m/1H/2H/4H] <br>UTC+8 opening price k-line: [6H/12H/1D/1W/1M]<br>UTC+0 opening price k-line: [6Hutc/12Hutc/1Dutc/1Wutc/1Mutc] |
| limit  | String | No       | Number of results per request. The maximum is `100`; The default is `100` |

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
        "1"
    ],
    [
        "1597026383085",
        "3.731",
        "3.799",
        "3.494",
        "3.72",
        "1"
    ]
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| ts         | String   | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| o          | String   | Open price                                    |
| h          | String   | highest price                                  |
| l          | String   | Lowest price                                  |
| c          | String   | Close price                                   |
| confirm       | String  | The state of candlesticks.<br>`0` represents that it is uncompleted, `1` represents that it is completed. |

<aside class="notice">
<p>The data returned will be arranged in an array like this: [ts,o,h,l,c,confirm] </p>
</aside>
