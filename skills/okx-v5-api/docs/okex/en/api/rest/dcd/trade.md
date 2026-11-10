---
site: okex
lang: en
name: POST / Trade
method: POST
path: /api/v5/finance/sfp/dcd/trade
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Trade

Places a dual investment order using a valid quote.

#### Rate Limit: 2 requests per 60 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/sfp/dcd/trade`

> Request Example

```shell
POST /api/v5/finance/sfp/dcd/trade
body
{
    "quoteId": "quoterbpDCD-QUOTE17732116652401234"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| quoteId | String | Yes | Quote ID |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "quoteId": "quoterbpDCD-QUOTE17732116652401234",
            "ordId": "987654321",
            "state": "live"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| quoteId | String | Quote ID |
| ordId | String | Order ID |
| state | String | Order state<br>`initial`: request has been received by system, will further process<br>`pending_book`: trade received by liquidity provider, pending further processing<br>`live`: trade is live<br>`rejected`: trade has been rejected |
