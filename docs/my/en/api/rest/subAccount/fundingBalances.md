---
site: my
lang: en
name: Get sub-account funding balance
method: GET
path: /api/v5/asset/subaccount/balances
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get sub-account funding balance

Query detailed balance info of Funding Account of a sub-account via the master account (applies to master accounts only)

#### Rate limit：6 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/asset/subaccount/balances`

> Request sample

```shell
GET /api/v5/asset/subaccount/balances?subAcct=test1

```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Get sub-account funding balance
result = subAccountAPI.get_funding_balance(
    subAcct="hahawang1"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                 |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | Yes     | Sub-account name  |
| ccy | String | No     | Single currency or multiple currencies (no more than 20) separated with comma, e.g. `BTC` or `BTC,ETH`.  |

> Returned result

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"availBal": "37.11827078",
			"bal": "37.11827078",
			"ccy": "ETH",
			"frozenBal": "0"
		}
	]
}
```

#### Response parameters

| Parameter    | Type   | Description           |
| :-------- | :----- | :------------- |
| ccy       | String | Currency |
| bal       | String | Balance  |
| frozenBal | String | Frozen balance |
| availBal  | String | Available balance |
