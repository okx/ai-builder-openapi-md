---
site: my
lang: en
name: Manual borrow / repay
method: POST
path: /api/v5/account/spot-manual-borrow-repay
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Manual borrow / repay

Only applicable to `Spot mode` (enabled borrowing)

#### Rate Limit: 1 request per 3 seconds
#### Rate limit rule: Master Account User ID

#### HTTP Request

`POST /api/v5/account/spot-manual-borrow-repay`

> Request Example

```shell
POST /api/v5/account/spot-manual-borrow-repay 
body
{
    "ccy":"USDT",
    "side":"borrow",
    "amt":"100"
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

result = accountAPI.spot_manual_borrow_repay(ccy="USDT", side="borrow", amt= "1")
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                 |
| :-------- | :----- |:---------|:----------------------------|
| ccy    | String | Yes      | Currency, e.g. `BTC`        |
| side    | String | Yes      | Side<br>`borrow`<br>`repay` |
| amt    | String | Yes      | Amount                      |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ccy":"USDT",
            "side":"borrow",
            "amt":"100"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                 |
|:----------| :----- |:----------------------------|
| ccy       | String | Currency, e.g. `BTC`        |
| side      | String | Side<br>`borrow`<br>`repay` |
| amt       | String | Actual amount               |
