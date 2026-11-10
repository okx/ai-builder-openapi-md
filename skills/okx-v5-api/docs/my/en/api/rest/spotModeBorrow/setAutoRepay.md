---
site: my
lang: en
name: Set auto repay
method: POST
path: /api/v5/account/set-auto-repay
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Set auto repay

Only applicable to `Spot mode` (enabled borrowing)

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-auto-repay`

> Request Example

```shell
POST /api/v5/account/set-auto-repay
body
{
    "autoRepay": true
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

result = accountAPI.set_auto_repay(autoRepay=True)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                        |
| :-------- | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------|
| autoRepay    | Boolean | Yes      | Whether auto repay is allowed or not under `Spot mode`<br>`true`: Enable auto repay<br>`false`: Disable auto repay |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "autoRepay": true
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                                                                                      |
|:----------| :----- |:-------------------------------------------------------------------------------------------------|
| autoRepay | Boolean | Whether auto repay is allowed or not under `Spot mode`<br>`true`: Enable auto repay<br>`false`: Disable auto repay |
