---
site: okex
lang: en
name: GET / Loan history
method: GET
path: /api/v5/finance/flexible-loan/loan-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Loan history

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/loan-history`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/loan-history
```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.loan_history()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                                                                                                             |
|:---------------|:----------|:-------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type           | String    | No           | Action type<br>`borrowed`<br>`repaid`<br>`collateral_locked`<br>`collateral_released`<br>`forced_repayment_buy`<br>`forced_repayment_sell`<br>`forced_liquidation`<br>`partial_liquidation`<br>`sell_collateral`<br>`buy_transition_coin`<br>`sell_transition_coin`<br>`buy_borrowed_coin` |
| after          | String    | No           | Pagination of data to return records earlier than the requested `refId`(not include)                                                                                                        |
| before         | String    | No           | Pagination of data to return records newer than the requested `refId`(not include)                                                                                                          |
| limit          | String    | No           | Number of results per request. The maximum is `100`. The default is `100`.                                                                                                                  |
| ordId        | String    | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will return data of all orders |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "amt": "-0.001",
            "ccy": "DOT",
            "refId": "17316594851045086",
            "ts": "1731659485000",
            "type": "collateral_locked"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type**             | **Description**                                                                     |
|:--------------|:---------------------|:------------------------------------------------------------------------------------|
| refId         | String               | Reference ID                                                                        |
| type          | String               | Action type                                                                         |
| ccy           | String               | Currency, e.g. `BTC`                                                                |
| amt           | String               | Amount                                                                              |
| ts           | String               | Timestamp for the action, Unix timestamp format in milliseconds, e.g. `1597026383085` |
