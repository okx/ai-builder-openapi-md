---
site: okex
lang: en
name: GET / Order state
method: GET
path: /api/v5/finance/sfp/dcd/order-status
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Order state

Returns the current state of a dual investment order.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/sfp/dcd/order-status`

> Request Example

```shell
GET /api/v5/finance/sfp/dcd/order-status?ordId=987654321
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
            "state": "live"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| ordId | String | Order ID |
| state | String | Order state<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
