---
site: okex
lang: en
name: POST / Redeem
method: POST
path: /api/v5/finance/sfp/dcd/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Redeem

Confirms early redemption using a valid redeem quote. This is step 2 of the two-step early redemption flow.

#### Rate Limit: 2 requests per 60 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/sfp/dcd/redeem`

> Request Example

```shell
POST /api/v5/finance/sfp/dcd/redeem
body
{
    "ordId": "987654321",
    "quoteId": "quoterbcDCD-REDEEM17732116652401234"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| ordId | String | Yes | Order ID |
| quoteId | String | Yes | Quote ID |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "state": "pending_redeem_booking"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| ordId | String | Order ID |
| state | String | order state<br>`pending_redeem_booking`: redeem received, waiting for liquidity provider further processing<br>`pending_redeem`: liquidity provider booked, waiting for transfer<br>`redeeming`: redemption in progress<br>`redeemed`: redemption completed |
