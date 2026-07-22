---
site: okex
lang: en
name: Get history of managed sub-account transfer
method: GET
path: /api/v5/asset/subaccount/managed-subaccount-bills
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get history of managed sub-account transfer

Only applicable to the trading team's master account to getting transfer records of managed sub accounts entrusted to oneself.

#### Rate limit：6 requests per second
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/asset/subaccount/managed-subaccount-bills`

> Request sample

```shell
GET /api/v5/asset/subaccount/managed-subaccount-bills
```

#### Request Parameters

| Parameter | Type  | Required | Description                                                                                                                       |
|:----------| :----- |:---------|:----------------------------------------------------------------------------------------------------------------------------------|
| ccy       | String | No       | Currency, e.g `BTC`                                                                                                               |
| type      | String | No       | Transfer type<br>`0`: Transfers from master account to sub-account<br>`1`: Transfers from sub-account to master account           |
| subAcct   | String | No       | Sub-account name                                                                                                                  |
| subUid    | String | No       | Sub-account UID                                                                                                                   |
| after     | String | No       | Query the data prior to the requested bill ID creation time (exclude), Unix timestamp in millisecond format, e.g. `1597026383085` |
| before    | String | No       | Query the data after the requested bill ID creation time (exclude), Unix timestamp in millisecond format, e.g. `1597026383085`    |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                            |

> Returned results

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"billId": "12344",
		"type": "1",
		"ccy": "BTC",
		"amt": "2",
		"subAcct": "test-1",
		"subUid": "xxxxxx",
		"ts": "1597026383085"
	}]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                                                   |
| :--------- | :------- |:----------------------------------------------------------------------------------|
| billId     | String   | Bill ID                                                                           |
| ccy      | String     | Transfer currency                                                                 |
| amt     | String      | Transfer amount                                                                   |
| type      | String    | Bill type                                                                         |
| subAcct    | String   | Sub-account name                                                                  |
| subUid    | String   | Sub-account UID                                                                   |
| ts         | String   | Bill ID creation time, Unix timestamp in millisecond format, e.g. `1597026383085` |
