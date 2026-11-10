---
site: okex
lang: en
name: POST / Repay
method: POST
path: /api/v5/finance/flexible-loan/repay
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Repay

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/flexible-loan/repay`

Repays part or all of an order's outstanding borrow amount. Settlement is asynchronous; a successful response means the request was accepted. Use the Loan info endpoint to confirm the final result.

> Request Example

```shell
POST /api/v5/finance/flexible-loan/repay
body
{
  "ordId": "680234819012345678",
  "ccy": "USDT",
  "amt": "1000",
  "clOrdId": "my-repay-001"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
|:--|:--|:--|:--|
| ordId | String | Yes | Order ID to repay. |
| ccy | String | Yes | Repayment currency. It must match the order's borrow currency. |
| amt | String | Yes | Repayment amount. Repaying the entire outstanding balance closes the order. |
| clOrdId | String | Yes | Client-supplied idempotency key, scoped to the UID and endpoint path. It is retained for 60 seconds; a completed duplicate returns `51016`, and an in-flight duplicate returns `51784`. |

> Response Example

```json
{
  "code": "0",
  "data": [{"ordId": "680234819012345678", "ccy": "USDT", "amt": "1000", "clOrdId": "my-repay-001"}],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
|:--|:--|:--|
| ordId | String | Order ID. |
| ccy | String | Repaid currency. |
| amt | String | Requested repayment amount, echoed from the request. |
| clOrdId | String | Client-supplied ID echoed from the request. |
