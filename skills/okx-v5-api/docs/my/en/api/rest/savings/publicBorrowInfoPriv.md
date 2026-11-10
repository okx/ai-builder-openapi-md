---
site: my
lang: en
name: GET / Public borrow info (private)
method: GET
path: /api/v5/finance/savings/lending-rate-summary-priv
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / Public borrow info (private)

This is a private endpoint and authentication is required for access.

Retrieve lending rate summary for the site of the user.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/finance/savings/lending-rate-summary-priv`

> Request Example

```shell
GET /api/v5/finance/savings/lending-rate-summary-priv

```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy            | String    | No           | Currency, e.g. `BTC` |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"avgAmt": "10000",
		"avgAmtUsd": "10000000000",
		"avgRate": "0.03",
		"preRate": "0.02",
		"estRate": "0.01"
	}]
}
```

#### Response Parameters

| Parameter    | Type   | Description                                 |
| :-------- | :----- |:--------------------------------------------|
| ccy       | String | Currency, e.g. `BTC`                        |
| avgAmt       | String | ~~24H average borrowing amount~~(deprecated)                |
| avgAmtUsd  | String | ~~24H average borrowing amount in `USD` value~~(deprecated) |
| avgRate  | String | 24-hours average annual borrowing rate                    |
| preRate  | String | Last annual borrowing interest rate                   |
| estRate  | String | Next estimate annual borrowing interest rate          |
