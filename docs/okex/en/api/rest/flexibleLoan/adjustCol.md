---
site: okex
lang: en
name: POST / Adjust collateral
method: POST
path: /api/v5/finance/flexible-loan/adjust-collateral
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Adjust collateral

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/flexible-loan/adjust-collateral`

> Request Example

```shell
POST /api/v5/finance/flexible-loan/adjust-collateral
body
{
    "type":"add",
    "ordId": "12345",
    "collateralCcy": "BTC",
    "collateralAmt": "0.1"
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
result = flexibleLoanAPI.adjust_collateral(type="add", ordId="12345", collateralCcy="USDT", collateralAmt="1")
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                        |
| :------------- |:----------|:-------------|:-----------------------------------------------------------------------|
| type          | String    | Yes          | Operation type<br>`add`: Add collateral<br>`reduce`: Reduce collateral |
| collateralCcy | String    | Yes          | Collateral currency, e.g. `BTC`                                        |
| collateralAmt | String    | Yes          | Collateral amount                                                      |
| ordId       | String    | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will assume it is acting against the existing order with the earliest order start time.<br>If there are no existing orders, system will return error `51063` |

> Response Example

```json
{
    "code": "0",
    "data": [
    ],
    "msg": ""
}
```

#### Response Parameters

code = `0` means your request has been accepted (It doesn't mean the request has been successfully handled.)
