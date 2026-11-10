---
site: us
lang: en
name: Get maximum withdrawals
method: GET
path: /api/v5/account/max-withdrawal
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get maximum withdrawals

Retrieve the maximum transferable amount from trading account to funding account. If no currency is specified, the transferable amount of all owned currencies will be returned.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-withdrawal`

> Request Example

```shell
GET /api/v5/account/max-withdrawal

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get maximum withdrawals
result = accountAPI.get_max_withdrawal()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description           |
| :----- | :----- | :------- | :------------- |
| ccy    | String | No       | Single currency or multiple currencies (no more than 20) separated with comma, e.g. `BTC` or `BTC,ETH`.  |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"ccy": "BTC",
			"maxWd": "124",
			"maxWdEx": "125",
			"spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
		},
		{
			"ccy": "ETH",
			"maxWd": "10",
			"maxWdEx": "12",
			"spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
		}
	]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                 |
| :--------- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy        | String   | Currency                                                                                                                                        |
| maxWd      | String   | Max withdrawal (excluding borrowed assets under `Spot mode`/`Multi-currency margin`/`Portfolio margin`)                                         |
| maxWdEx      | String   | Max withdrawal (including borrowed assets under `Spot mode`/`Multi-currency margin`/`Portfolio margin`)                                         |
| spotOffsetMaxWd      | String   | Max withdrawal under Spot-Derivatives risk offset mode (excluding borrowed assets under `Portfolio margin`)<br>Applicable to `Portfolio margin` |
| spotOffsetMaxWdEx     | String   | Max withdrawal under Spot-Derivatives risk offset mode (including borrowed assets under `Portfolio margin`)<br>Applicable to `Portfolio margin` |
