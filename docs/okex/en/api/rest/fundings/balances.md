---
site: okex
lang: en
name: Get balance
method: GET
path: /api/v5/asset/balances
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get balance

Retrieve the funding account balances of all the assets and the amount that is available or on hold.

<aside class="notice">
Only asset information of a currency with a balance greater than 0 will be returned.
</aside>  

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/balances`

> Request Example

```shell
GET /api/v5/asset/balances

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Get balane
result = fundingAPI.get_balances()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy            | String    | No           | Single currency or multiple currencies (no more than 20) separated with comma, e.g. `BTC` or `BTC,ETH`. |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [
		{
			"availBal": "37.11827078",
			"bal": "37.11827078",
			"ccy": "ETH",
			"frozenBal": "0"
		}
	]
}
```

#### Response Parameters

| Parameter    | Type   | Description           |
| :-------- | :----- | :------------- |
| ccy       | String | Currency |
| bal       | String | Balance  |
| frozenBal | String | Frozen balance |
| availBal  | String | Available balance |
