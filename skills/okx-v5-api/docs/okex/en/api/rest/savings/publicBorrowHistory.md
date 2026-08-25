---
site: okex
lang: en
name: GET / Public borrow history (public)
method: GET
path: /api/v5/finance/savings/lending-rate-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Public borrow history (public)

Authentication is not required for this public endpoint.<br>
Only returned records after December 14, 2021.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/finance/savings/lending-rate-history`

> Request Example

```shell
GET /api/v5/finance/savings/lending-rate-history

```

```python
import okx.Finance.Savings as Savings

flag = "0"  # Production trading:0 , demo trading:1

SavingsAPI = Savings.SavingsAPI(flag=flag)

result = SavingsAPI.get_public_borrow_history()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                                                                                                |
| :------------- | :-------- | :----------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy            | String    | No           | Currency, e.g. `BTC`                                                                                                                                                           |
| after          | String | No       | Pagination of data to return records earlier than the requested `ts`, Unix timestamp format in milliseconds, e.g. `1597026383085`                                              |
| before         | String | No       | Pagination of data to return records newer than the requested `ts`, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                |
| limit          | String | No      | Number of results per request. The maximum is `100`. The default is `100`.<br>If `ccy` is not specified, all data under the same `ts` will be returned, not limited by `limit` |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"amt": "0.01",
		"rate": "0.001",
		"lendingRate": "0.001",
		"ts": "1597026383085"
	}]
}
```

#### Response Parameters

| Parameter    | Type   | Description                                                    |
| :-------- | :----- |:------------------------------------------------------------------|
| ccy       | String | Currency, e.g. `BTC`                                              |
| amt       | String | ~~Lending amount~~(deprecated)                            		 |
| rate  | String | Annual borrowing interest rate                                        |
| lendingRate | String | Annual lending interest rate                                      |
| ts  | String | Time, Unix timestamp format in milliseconds, e.g. `1597026383085` 		 |
