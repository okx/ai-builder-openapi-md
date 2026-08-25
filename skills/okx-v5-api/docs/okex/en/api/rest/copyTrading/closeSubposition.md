---
site: okex
lang: en
name: POST / Close lead position
method: POST
path: /api/v5/copytrading/close-subposition
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Close lead position

You can only close a lead position once a time. <br>
It is required to pass subPosId which can get from [Get existing leading positions](/docs-v5/en/#order-book-trading-copy-trading-get-existing-lead-positions).

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`POST /api/v5/copytrading/close-subposition`

> Request example

```
POST /api/v5/copytrading/close-subposition
body
{
    "subPosId": "518541406042591232",
}
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                             |
| :------ | :----- |:-----|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`, the default value                                                                                                                                                                  |
| subPosId  | String | Yes    | Lead position ID                                                                                                                                                                                                        |
| ordType     | String | No       | Order type<br>`market`：Market order, the default value<br>`limit`：Limit order<br>                                                                                                                                       |
| px     | String | No        | Order price. Only applicable to `limit` order and `SPOT` lead trader <br>If the price is 0, the pending order will be canceled. <br>It is modifying order if you set `px` after placing limit order.                    |
| tag       | String | No      | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                         |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "subPosId": "518560559046594560",
            "tag":""
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                        |
| :---------- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subPosId      | String   | Lead position ID                                                                                                                                                                                       |
| tag      | String   | Order tag                                                                                                                                                                                              |
