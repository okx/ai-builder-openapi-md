---
site: us
lang: en
name: Set greeks (PA/BS)
method: POST
path: /api/v5/account/set-greeks
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Set greeks (PA/BS)

Set the display type of Greeks.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-greeks`

> Request Example

```shell
POST /api/v5/account/set-greeks 
body
{
    "greeksType":"PA"
}

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Set greeks (PA/BS)
result = accountAPI.set_greeks(greeksType="PA")
print(result)
```

#### Request Parameters

| Parameter     | Type   | Required | Description                                           |
| :--------- | :----- | :------- | :--------------------------------------------- |
| greeksType | String | Yes       | Display  type of Greeks.<br>`PA`: Greeks in coins <br>`BS`: Black-Scholes Greeks in dollars |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"greeksType": "PA"
	}]
}
```

#### Response Parameters

| Parameter     | Type   | Description                 |
| :--------- | :----- | :------------------- |
| greeksType | String | Display type of Greeks. |
