---
site: okex
lang: en
name: GET / Order history
method: GET
path: /api/v5/finance/sfp/dcd/order-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Order history

Return dual investment history orders

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/sfp/dcd/order-history`

> Request Example

```shell
GET /api/v5/finance/sfp/dcd/order-history
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| ordId | String | No | Order ID. When provided, returns that specific order directly (ignores other filters) |
| productId | String | No | Product ID, e.g. `BTC-USDT-260327-77000-C` |
| uly | String | No | Underlying index, e.g. `BTC-USD` |
| state | String | No | Order state filter<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
| beginId | String | No | Return records newer than this order ID |
| endId | String | No | Return records earlier than this order ID |
| begin | String | No | Begin timestamp filter, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| end | String | No | End timestamp filter, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| limit | String | No | Number of results per request, max 100 |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "quoteId": "quoterbpDCD-QUOTE17732116652401234",
            "state": "settled",
            "productId": "BTC-USDT-260327-77000-C",
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "uly": "BTC-USD",
            "strike": "77000",
            "notionalSz": "1.5",
            "notionalCcy": "BTC",
            "absYield": "0.00806038",
            "annualizedYield": "0.1834",
            "yieldSz": "0.01209057",
            "yieldCcy": "BTC",
            "settleSz": "1.51209057",
            "settleCcy": "BTC",
            "settlePx": "76500",
            "settleTime": "1774598400000",
            "expTime": "1774598400000",
            "redeemStartTime" : "1774598400000",
            "redeemEndime": "1774598400000",
            "cTime": "1773212400000",
            "uTime": "1773212400000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| ordId | String | Order ID |
| quoteId | String | Quote ID |
| state | String | Order state<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
| productId | String | Product ID, e.g. `BTC-USDT-260327-77000-C` |
| baseCcy | String | Base currency, e.g. `BTC` |
| quoteCcy | String | Quote currency, e.g. `USDT` |
| uly | String | Underlying index, e.g. `BTC-USD` |
| strike | String | Strike price |
| notionalSz | String | Notional size |
| notionalCcy | String | Notional currency |
| absYield | String | Absolute yield rate |
| annualizedYield | String | Annual yield rate |
| yieldSz | String | Yield size |
| yieldCcy | String | Yield currency |
| settleSz | String | Settlement size ("" if not yet settled) |
| settleCcy | String | Settlement currency ("" if not yet settled) |
| settlePx | String | Settlement price ("" if not yet settled) |
| expTime | String | Product expiration time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| settleTime | String | Actual settled time, Unix timestamp format in milliseconds, e.g. `1597026383085` ("" if not yet settled) |
| redeemStartTime | String | Earliest time to request early redemption, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| redeemEndTime | String | Latest time to request early redemption, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| cTime | String | Order creation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime | String | Last update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
