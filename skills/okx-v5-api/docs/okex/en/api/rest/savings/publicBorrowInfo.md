---
site: okex
lang: en
name: GET / Public borrow info (public)
method: GET
path: /api/v5/finance/savings/lending-rate-summary
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Public borrow info (public)

Authentication is not required for this public endpoint.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/finance/savings/lending-rate-summary`

> Request Example

```shell
GET /api/v5/finance/savings/lending-rate-summary

```

```python
import okx.Finance.Savings as Savings

flag = "0"  # Production trading:0 , demo trading:1

SavingsAPI = Savings.SavingsAPI(flag=flag)

result = SavingsAPI.get_public_borrow_info()
print(result)
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
