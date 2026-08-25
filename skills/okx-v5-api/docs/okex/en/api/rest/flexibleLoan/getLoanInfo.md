---
site: okex
lang: en
name: GET / Loan info
method: GET
path: /api/v5/finance/flexible-loan/loan-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Loan info

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/loan-info`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/loan-info
```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.loan_info()
print(result)
```

#### Request Parameters

| **Parameters** | **Types**           | **Required** | **Description**                 |
|:---------------|:--------------------|:-------------|:--------------------------------|
| ordId               | String    | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will return data of all existing orders |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "12345",
            "collateralData": [
                {
                    "amt": "0.0000097",
                    "ccy": "COMP"
                },
                {
                    "amt": "0.78",
                    "ccy": "STX"
                },
                {
                    "amt": "0.001",
                    "ccy": "DOT"
                },
                {
                    "amt": "0.05357864",
                    "ccy": "LUNA"
                }
            ],
            "collateralNotionalUsd": "1.5078763",
            "curLTV": "0.5742",
            "liqLTV": "0.8374",
            "loanData": [
                {
                    "amt": "0.86590608",
                    "ccy": "USDC"
                }
            ],
            "loanNotionalUsd": "0.8661285",
            "marginCallLTV": "0.7374",
            "riskWarningData": {
                "instId": "",
                "liqPx": ""
            }
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**     | **Type**        | **Description**                                                                                                                                                                                                                                                    |
|:------------------|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ordId           | String | Order ID                                                                                                                                                                                                                                                                    |
| loanNotionalUsd       | String | Loan value in `USD`                                                                                                                                                                                                                                                |
| loanData              | Array of objects | Loan data                                                                                                                                                                                                                                                          |
| > ccy                 | String | Loan currency, e.g. `USDT`                                                                                                                                                                                                                                         |
| > amt                 | String | Loan amount                                                                                                                                                                                                                                                        |
| collateralNotionalUsd | String | Adjusted collateral value in `USD`                                                                                                                                                                                                                                 |
| collateralData        | Array of objects | Collateral data                                                                                                                                                                                                                                                    |
| > ccy                 | String | Collateral currency, e.g. `BTC`                                                                                                                                                                                                                                    |
| > amt                 | String | Collateral amount                                                                                                                                                                                                                                                  |
| riskWarningData       | Object | Risk warning data                                                                                                                                                                                                                                                  |
| > instId              | String | Liquidation instrument ID, e.g. `BTC-USDT`<br>This field is only valid when there is only one type of collateral and one type of borrowed currency. In other cases, it returns "".                                                                                 |
| > liqPx               | String | Liquidation price<br>The unit of the liquidation price is the quote currency of the instrument, e.g. `USDT` in `BTC-USDT`.<br>This field is only valid when there is only one type of collateral and one type of borrowed currency. In other cases, it returns "". |
| curLTV                | String | Current LTV, e.g. `0.1` represents `10%`<br>Note: LTV = Loan to Value                                                                                                                                                                                              |
| marginCallLTV         | String | Margin call LTV, e.g. `0.1` represents `10%`<br>If your loan hits the margin call LTV, our system will automatically warn you that your loan is getting close to forced liquidation.                                                                               |
| liqLTV                | String | Liquidation LTV, e.g. `0.1` represents `10%`<br>If your loan reaches liquidation LTV, it'll trigger forced liquidation. When this happens, you'll lose access to your collateral and any repayments made.                                                          |
