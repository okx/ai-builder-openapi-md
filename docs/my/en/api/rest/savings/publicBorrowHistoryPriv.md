---
site: my
lang: en
name: GET / Public borrow history (private)
method: GET
path: /api/v5/finance/savings/lending-rate-history-priv
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / Public borrow history (private)

This is a private endpoint and authentication is required for access.

Retrieve the public borrow history for the site of the user.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/savings/lending-rate-history-priv`

> Request Example

```shell
GET /api/v5/finance/savings/lending-rate-history-priv

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
| ts  | String | Time, Unix timestamp format in milliseconds, e.g. `1597026383085` 		 |
