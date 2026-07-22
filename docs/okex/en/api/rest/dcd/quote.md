---
site: okex
lang: en
name: POST / Request for quote
method: POST
path: /api/v5/finance/sfp/dcd/quote
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Request for quote

Requests a real-time quote for a dual investment product. The quote has a TTL and must be used before expiry.

#### Rate Limit: 10 requests per 60 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/sfp/dcd/quote`

> Request Example

```shell
POST /api/v5/finance/sfp/dcd/quote
body
{
    "productId": "BTC-USDT-260327-77000-C",
    "notionalSz": "1.5",
    "notionalCcy": "BTC"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| productId | String | Yes | Product ID |
| notionalSz | String | Yes | Investment size |
| notionalCcy | String | Yes | Investment currency |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "absYield": "0.00135182",
            "annualizedYield": "69.65",
            "interestAccrualTime": "1773241200000",
            "notionalSz": "0.001",
            "notionalCcy": "BTC",
            "productId": "BTC-USDT-260312-72000-C",
            "quoteId": "qtbcDCD-QUOTE17732395560537636",
            "validUntil": "1774584000000",
            "idxPx": "69000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| absYield | String | Absolute yield |
| annualizedYield | String | Annualized yield |
| interestAccrualTime | String | Interest accrual start time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| notionalSz | String | Investment size |
| notionalCcy | String | Investment currency |
| productId | String | Product ID |
| quoteId | String | Quote ID |
| validUntil | String | Quote valid until, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| idxPx | String | Index price |
