---
site: okex
lang: en
name: POST / Savings purchase/redemption
method: POST
path: /api/v5/finance/savings/purchase-redempt
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Savings purchase/redemption

Only the assets in the funding account can be used for saving.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/savings/purchase-redempt`

> Request Example

```shell
POST /api/v5/finance/savings/purchase-redempt
body
{
    "ccy":"BTC",
    "amt":"1",
    "side":"purchase",
    "rate":"0.01"
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

result = SavingsAPI.savings_purchase_redemption(ccy='USDT',amt="0.1",side="purchase",rate="1")
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                                                                                                                                                  |
|-----------| ------ |-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy       | String | Yes         | Currency, e.g. `BTC`                                                                                                                                                                                                                         |
| amt       | String | Yes         | Purchase/redemption amount                                                                                                                                                                                                                   |
| side      | String | Yes         | Action type. <br>`purchase`: purchase saving shares <br>`redempt`: redeem saving shares                                                                                                                                                      |
| rate      | String | Conditional | Annual purchase rate, e.g. `0.1` represents `10%`<br>Only applicable to purchase saving shares<br>The interest rate of the new subscription will cover the interest rate of the last subscription<br>The rate value range is between 1% and 365% |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ccy":"BTC",
            "amt":"1",
            "side":"purchase",
            "rate": "0.01"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                |
| ------ | ------ |----------------------------|
| ccy    | String | Currency                   |
| amt    | String | Purchase/Redemption amount |
| side   | String | Action type                |
| rate   | String | Annual purchase rate, e.g. `0.1` represents `10%`     |
