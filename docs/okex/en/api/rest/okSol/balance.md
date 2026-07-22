---
site: okex
lang: en
name: GET / Balance
method: GET
path: /api/v5/finance/staking-defi/sol/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Balance

The balance represents the real-time total OKSOL holdings across the entire account, including assets in the trading account, funding account, and those currently in the redeeming process.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/sol/balance`

> Request Example

```shell
GET /api/v5/finance/staking-defi/sol/balance
```

```python
import okx.Finance.SolStaking as SolStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = SolStaking.SolStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.sol_balance()
print(result)
```

#### Request Parameters

None

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0.01100012",
            "ccy": "OKSOL",
            "latestInterestAccrual": "0.00000012",
            "totalInterestAccrual": "0.00000012"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter             | Type   | Description             |
|-----------------------|--------|-------------------------|
| ccy                   | String | Currency, e.g. `OKSOL`  |
| amt                   | String | Currency amount         |
| latestInterestAccrual | String | Latest interest accrual |
| totalInterestAccrual  | String | Total interest accrual  |
