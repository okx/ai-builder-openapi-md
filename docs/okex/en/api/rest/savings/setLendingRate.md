---
site: okex
lang: en
name: POST / Set lending rate
method: POST
path: /api/v5/finance/savings/set-lending-rate
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Set lending rate

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/savings/set-lending-rate`

> Request Example

```shell
POST /api/v5/finance/savings/set-lending-rate
body
{
    "ccy":"BTC",
    "rate":"0.02"
}

```

```python
import okx.Finance.Savings as Savings

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.set_lending_rate(ccy='USDT',rate="1")
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                        |
| ------ | ------ | -------- |--------------------------------------------------------------------|
| ccy    | String | Yes       | Currency, e.g. `BTC`                                               |
| rate   | String | Yes       | Annual lending rate<br>The rate value range is between 1% and 365% |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"rate": "0.02"
	}]
}
```

#### Response Parameters

| Parameter | Type   | Description          |
| ------ | ------ |----------------------|
| ccy    | String | Currency, e.g. `BTC` |
| rate   | String | Annual lending rate  |
