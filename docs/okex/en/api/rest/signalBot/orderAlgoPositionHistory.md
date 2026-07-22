---
site: okex
lang: en
name: GET / Position history
method: GET
path: /api/v5/tradingBot/signal/positions-history
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Position history

Retrieve the updated position data for the last 3 months. Return in reverse chronological order using utime.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/positions-history`

> Request Example

```shell
GET /api/v5/tradingBot/signal/positions-history?algoId=1234
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                                                                      |
| instId    | String | No       | Instrument ID, e.g.：`BTC-USD-SWAP`                                                                |
| after     | String | No       | Pagination of data to return records earlier than the requested `uTime`, Unix timestamp format in milliseconds, e.g.`1597026383085`            |
| before    | String | No       | Pagination of data to return records newer than the requested `uTime`, Unix timestamp format in milliseconds, e.g `1597026383085`              |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                         |

> Response Example

```json
{
  "code": "0",
  "data": [
    {
      "cTime": "1704724451471",
      "closeAvgPx": "200",
      "direction": "net",
      "instId": "ETH-USDT-SWAP",
      "lever": "5.0",
      "mgnMode": "cross",
      "openAvgPx": "220",
      "pnl": "-2.021",
      "pnlRatio": "-0.4593181818181818",
      "uTime": "1704724456322",
      "uly": "ETH-USDT"
    }
  ],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| instId            | String | Instrument ID                                                                                |
| mgnMode	        | String | Margin mode `cross` `isolated`                                                            |
| cTime             | String | Created time of position                                                                  |
| uTime             | String | Updated time of position                                                                  |
| openAvgPx         | String | Average price of opening position                                                         |
| closeAvgPx        | String | Average price of closing position                                                         |
| pnl               | String | Profit and loss                                                                           |
| pnlRatio          | String | P&L ratio                                                                                 |
| lever             | String | Leverage                                                                                  |
| direction         | String | Direction: `long` `short`                                                                 |
| uly               | String | Underlying                                                                                |                                                                          |
