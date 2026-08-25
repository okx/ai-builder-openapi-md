---
site: okex
lang: en
name: GET / Maximum collateral redeem amount
method: GET
path: /api/v5/finance/flexible-loan/max-collateral-redeem-amount
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Maximum collateral redeem amount

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/max-collateral-redeem-amount`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/max-collateral-redeem-amount?ccy=USDT&ordId=12345

```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.max_collateral_redeem_amount(ordId="12345", ccy="USDT")
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                 |
|:---------------|:----------|:-------------|:--------------------------------|
| ccy            | String    | Yes          | Collateral currency, e.g. `USDT` |
| ordId        | String    | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will assume it is acting against the existing order with the earliest order start time.<br>If there are no existing orders, system will return empty result data. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "maxRedeemAmt": "1"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type**        | **Description**           |
|:--------------|:----------------|:--------------------------|
| ccy           | String | Collateral currency, e.g. `USDT`    |
| maxRedeemAmt  | String | Maximum collateral redeem amount    |
