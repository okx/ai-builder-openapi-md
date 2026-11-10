---
site: turkey
lang: en
name: Get history of sub-account transfer
method: GET
path: /api/v5/asset/subaccount/bills
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get history of sub-account transfer

This endpoint is only available for master accounts. Transfer records are available from September 28, 2022 onwards.

#### Rate limit：6 requests per second
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/asset/subaccount/bills`

> Request sample

```shell
GET /api/v5/asset/subaccount/bills
```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Get history of sub-account transfer
result = subAccountAPI.bills()
print(result)
```

#### Request Parameters

| Parameter | Type  | Required | Description                                                                                                                                                                                                                                                     |
|:----------| :----- |:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy       | String | No       | Currency, such as BTC                                                                                                                                                                                                                                           |
| type      | String | No       | Transfer type<br>`0`: Transfers from master account to sub-account<br>`1` : Transfers from sub-account to master account.                                                                                                                                       |
| subAcct   | String | No       | Sub-account name                                                                                                                                                                                                                                                |
| after     | String | No       | Query the data prior to the requested bill ID creation time (exclude), the value should be a Unix timestamp in millisecond format. e.g. `1597026383085`                                                                                                         |
| before    | String | No       | Query the data after the requested bill ID creation time (exclude), the value should be a Unix timestamp in millisecond format. e.g. `1597026383085`                                                                                                            |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                                                                                                                                          |

> Returned results

```json
{
	"code": "0",
	"msg": "",
	"data": [
      {
        "amt": "1.1",
        "billId": "89887685",
        "ccy": "USDT", 
        "subAcct": "hahatest1",
        "ts": "1712560959000",
        "type": "0"
      }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                                                   |
|:-------------------|:---------|:----------------------------------------------------------------------------------|
| billId             | String   | Bill ID                                                                           |
| ccy                | String   | Transfer currency                                                                 |
| amt                | String   | Transfer amount                                                                   |
| type               | String   | Bill type                                                                         |
| subAcct            | String   | Sub-account name                                                                  |
| ts                 | String   | Bill ID creation time, Unix timestamp in millisecond format, e.g. `1597026383085` |
