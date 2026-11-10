---
site: my
lang: en
name: GET / Saving balance
method: GET
path: /api/v5/finance/savings/balance
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / Saving balance

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/savings/balance`

> Request Example

```shell
GET /api/v5/finance/savings/balance?ccy=USDT

```

```python
import okx.Finance.Savings as Savings

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.get_saving_balance(ccy="USDT")
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy            | String    | No           | Currency, e.g. `BTC` |

> Response Example

```json
{
    "code": "0",
	"msg":"",
    "data": [
        {
            "earnings": "0.0010737388791526",
            "redemptAmt": "",
            "rate": "0.0100000000000000",
            "ccy": "USDT",
            "amt": "11.0010737453457821",
            "loanAmt": "11.0010630707982819",
            "pendingAmt": "0.0000106745475002"
        }
    ]
}
```

#### Response Parameters

| Parameter    | Type   | Description                               |
| :-------- | :----- |:------------------------------------------|
| ccy       | String | Currency                                  |
| amt       | String | Currency amount                           |
| earnings  | String | Currency earnings           |
| rate  | String | Minimum annual lending rate configured by users  |
| loanAmt  | String | Lending amount                            |
| pendingAmt  | String | Pending amount                            |
| redemptAmt  | String | <del>Redempting amount</del> (Deprecated) |
