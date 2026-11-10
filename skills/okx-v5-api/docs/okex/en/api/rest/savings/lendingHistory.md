---
site: okex
lang: en
name: GET / Lending history
method: GET
path: /api/v5/finance/savings/lending-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Lending history

Return data in the past month.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/savings/lending-history`

> Request Example

```shell
GET /api/v5/finance/savings/lending-history

```

```python
import okx.Finance.Savings as Savings

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.get_lending_history()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy            | String    | No           | Currency, e.g. `BTC` |
| after          | String | No       | Pagination of data to return records earlier than the requested `ts`, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| before         | String | No       | Pagination of data to return records newer than the requested `ts`, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| limit          | String | No      | Number of results per request. The maximum is `100`. The default is `100`. |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"ccy": "BTC",
			"amt": "0.01",
			"earnings": "0.001",
			"rate": "0.01",
			"ts": "1597026383085"
		},
		{
			"ccy": "ETH",
			"amt": "0.2",
			"earnings": "0.001",
			"rate": "0.01",
			"ts": "1597026383085"
		}
	]
}
```

#### Response Parameters

| Parameter    | Type   | Description           |
| :-------- | :----- | :------------- |
| ccy       | String | Currency, e.g. `BTC` |
| amt       | String | Lending amount |
| earnings  | String | Currency earnings |
| rate  | String | Lending annual interest rate |
| ts  | String | Lending time, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
