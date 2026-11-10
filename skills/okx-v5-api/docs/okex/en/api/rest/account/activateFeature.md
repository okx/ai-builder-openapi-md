---
site: okex
lang: en
name: Activate feature
method: POST
path: /api/v5/account/activate-feature
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Activate feature

If order placement returns error code `54109`, call the following endpoint to activate USDC trading for the account; otherwise, you do not need to call this endpoint.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/activate-feature`

> Request example

```shell
POST /api/v5/account/activate-feature
body
{
    "feature": "1"
}
```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| feature | String | Yes | Feature to activate<br>`1`: USDC order book trading.<br>Call this endpoint only when order placement returns error code `54109`; otherwise, you do not need to call it.<br>Error code `51773` only indicates that this activation feature is not supported. Whether you can trade `Crypto-USDC` instruments depends on whether an order can be placed successfully.<br>Activation is shared between master accounts and sub-accounts. The master account or any of its sub-accounts only needs to call this endpoint once. |

> Response example

```json
{
    "code": "0",
    "msg": "",
    "data": []
}
```

#### Response parameters

None
