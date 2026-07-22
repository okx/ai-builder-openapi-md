---
site: okex
lang: en
name: POST / Maximum loan amount
method: POST
path: /api/v5/finance/flexible-loan/max-loan
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Maximum loan amount

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/flexible-loan/max-loan`

> Request Example

```shell
POST /api/v5/finance/flexible-loan/max-loan
body
{
    "ordId": "12345",
    "borrowCcy": "USDT"
}

```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.max_loan(ordId="12345", borrowCcy="USDT")
print(result)
```

#### Request Parameters

| **Parameters** | **Types**           | **Required** | **Description**                 |
|:---------------|:--------------------|:-------------|:--------------------------------|
| borrowCcy      | String              | Yes          | Currency to borrow, e.g. `USDT` |
| ordId        | String              | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will assume it is acting against the existing order with the earliest order start time.<br>If there are no existing orders, system will return empty result data. |
| supCollateral  | Array of objects     | No           | Supplementary collateral assets |
| > ccy          | String              | Yes           | Currency, e.g. `BTC`            |
| > amt          | String              | Yes           | Amount                          |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "borrowCcy": "USDT",
            "maxLoan": "0.01113",
            "notionalUsd": "0.01113356",
            "remainingQuota": "3395000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**     | **Type**        | **Description**           |
|:------------------|:----------------|:--------------------------|
| borrowCcy         | String | Currency to borrow, e.g. `USDT`    |
| maxLoan           | String | Maximum available loan    |
| notionalUsd       | String | Maximum available loan notional value, unit in `USD`    |
| remainingQuota    | String | Remaining quota, unit in `borrowCcy`    |
