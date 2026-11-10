---
site: okex
lang: en
name: POST / Borrow
method: POST
path: /api/v5/finance/flexible-loan/borrow
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Borrow

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/flexible-loan/borrow`

Creates a Flexible Loan order or adds a borrow amount to an existing active order.

> Request Example

```shell
POST /api/v5/finance/flexible-loan/borrow
body
{
  "loanData": {"ccy": "USDT", "amt": "1000"},
  "collateralData": [{"ccy": "BTC", "amt": "0.02"}],
  "eMode": "0",
  "clOrdId": "my-borrow-001"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
|:--|:--|:--|:--|
| ordId | String | No | Existing order ID. Omit it to create an order. |
| loanData | Object | Yes | Borrow currency and amount. |
| > ccy | String | Yes | Borrow currency. |
| > amt | String | Yes | Borrow amount. When `ordId` is provided, this is the additional amount. |
| collateralData | Array of objects | Conditional | Collateral assets. Required with 1–10 entries when creating an order; optional when adding to an order to add collateral together with the borrow. |
| > ccy | String | Yes | Collateral currency. |
| > amt | String | Yes | Collateral amount. |
| eMode | String | Conditional | `0`: disabled (default); `1`: enabled. Accepted only when creating an order. Every collateral/borrow pair must be supported by the E-Mode info endpoint. |
| clOrdId | String | Yes | Client-supplied ID. The idempotency key is scoped to the UID and endpoint path. It is retained for 60 seconds; a completed duplicate returns `51016`, and an in-flight duplicate returns `51784`. |

When `ordId` is provided, `eMode` must not be provided; E-Mode status is fixed at order creation. `collateralData` is optional and adds collateral to the existing order. For an E-Mode order, the added collateral must comply with the order's E-Mode pair.

> Response Example

```json
{
  "code": "0",
  "data": [{"ordId": "680234819012345678", "eMode": "0", "clOrdId": "my-borrow-001"}],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
|:--|:--|:--|
| ordId | String | Created order ID, or the order ID to which the borrow was added. |
| eMode | String | E-Mode status of the order. |
| clOrdId | String | Client-supplied ID echoed from the request. |
