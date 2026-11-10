---
site: okex
lang: en
name: GET / Borrowable currencies
method: GET
path: /api/v5/finance/flexible-loan/borrow-currencies
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Borrowable currencies

Get borrowable currencies

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/borrow-currencies`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/borrow-currencies
```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.borrow_currencies()
print(result)
```

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "borrowCcy": "USDT"
        },
        {
            "borrowCcy": "USDC"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                                               |
|:----------------| :------- |:--------------------------------------------------------------|
| borrowCcy       | String     |  Borrowable currency, e.g. `BTC`     |
