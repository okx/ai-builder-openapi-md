---
site: my
lang: en
name: Get interest rate
method: GET
path: /api/v5/account/interest-rate
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get interest rate

Get the user's current leveraged currency borrowing market interest rate

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/interest-rate`

> Request Example

```shell
GET /api/v5/account/interest-rate
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get the user's current leveraged currency borrowing interest rate
result = accountAPI.get_interest_rate()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                         |
| :------------- | :-------- | :----------- | :------------------------------------------------------------------------------------------------------ |
| ccy            | String    | No           | Currency, e.g. `BTC`  |

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ccy":"BTC",
            "interestRate":"0.0001"
        },
        {
            "ccy":"LTC",
            "interestRate":"0.0003"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                     |
| :-------- | :----- |:--------------------------------|
| interestRate     | String | Hourly borrowing interest rate |
| ccy               | String | Currency                        |
