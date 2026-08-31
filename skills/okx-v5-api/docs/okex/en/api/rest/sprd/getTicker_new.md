---
site: okex
lang: en
name: Get ticker (Public)
method: GET
path: /api/v5/market/sprd-ticker
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get ticker (Public)

Retrieve the latest price snapshot, best bid/ask price and quantity.

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/sprd-ticker`

> Request Example

```shell
GET /api/v5/market/sprd-ticker?sprdId=BTC-USDT_BTC-USDT-SWAP

```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| sprdId | String | Yes | spread ID, e.g. BTC-USDT_BTC-USDT-SWAP |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "sprdId": "BTC-USDT_BTC-USDT-SWAP",
            "last": "14.5",
            "lastSz": "0.5",
            "askPx": "8.5",
            "askSz": "12.0",
            "bidPx": "0.5",
            "bidSz": "12.0",
            "open24h": "4",
            "high24h": "14.5",
            "low24h": "-2.2",
            "vol24h": "6.67",
            "ts": "1715331406485"
        }
    ]
}

```

#### Response Parameters
| Parameter | Type | Description |
| --- | --- | --- |
| sprdId | String | spread ID |
| last | String | Last traded price |
| lastSz | String | Last traded size |
| askPx | String | Best ask price |
| askSz | String | Best ask size |
| bidPx | String | Best bid price |
| bidSz | String | Best bid size |
| open24h | String | Open price in the past 24 hours |
| high24h | String | Highest price in the past 24 hours |
| low24h | String | Lowest price in the past 24 hours |
| vol24h | String | 24h trading volume<br> The unit is USD for inverse spreads, and the corresponding baseCcy for linear and hybrid spreads. |
| ts | String | Ticker data generation time, Unix timestamp format in milliseconds, e.g. 1597026383085. |
