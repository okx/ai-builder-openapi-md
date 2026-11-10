---
site: okex
lang: en
name: POST / Request for redeem quote
method: POST
path: /api/v5/finance/sfp/dcd/redeem-quote
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Request for redeem quote

Requests an early redemption quote for a live dual investment order. This is step 1 of the two-step early redemption flow; call POST / Redeem to confirm.

#### Rate Limit: 10 requests per 60 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/sfp/dcd/redeem-quote`

> Request Example

```shell
POST /api/v5/finance/sfp/dcd/redeem-quote
body
{
    "ordId": "987654321"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| ordId | String | Yes | Order ID |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "quoteId": "quoterbcDCD-REDEEM17732116652401234",
            "redeemCcy": "BTC",
            "redeemSz": "1.4856",
            "termRate": "-0.50",
            "validUntil": "1774598400000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| ordId | String | Order ID |
| quoteId | String | Quote ID |
| redeemSz | String | Redeem size |
| redeemCcy | String | Redeem currency |
| termRate | String | Term rate |
| validUntil | String | Redeem quote valid until, Unix timestamp format in milliseconds, e.g. `1597026383085` |
