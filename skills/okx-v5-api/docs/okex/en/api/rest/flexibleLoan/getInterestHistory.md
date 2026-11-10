---
site: okex
lang: en
name: GET / Accrued interest
method: GET
path: /api/v5/finance/flexible-loan/interest-accrued
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Accrued interest

Retrieves the interest accrual history for flexible loans over the past 30 days.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/interest-accrued`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/interest-accrued
```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.interest_accrued()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required**  | **Description**                             |
|:---------------|:----------|:--------------|:--------------------------------------------|
| ccy            | String    | No            | Loan currency, e.g. `BTC`                   |
| after          | String    | No            | Pagination of data to return records earlier than the requested `refId`(not include)                                                                                                     |
| before         | String    | No            | Pagination of data to return records newer than the requested `refId`(not include)                                                                                                       |
| limit          | String    | No            | Number of results per request. The maximum is `100`. The default is `100`.  |
| ordId        | String    | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will return data of all orders |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDC",
            "interest": "0.00004054",
            "interestRate": "0.41",
            "loan": "0.86599309",
            "refId": "17319133035195744",
            "ts": "1731913200000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type**      | **Description**                                                                               |
|:--------------|:--------------|:----------------------------------------------------------------------------------------------|
| refId         | String        | Reference ID                                                                                  |
| ccy            | String        | Loan currency, e.g. `BTC`                                                                     |
| loan           | String        | Loan when calculated interest                                                                                    |
| interest       | String        | Interest                                                                                      |
| interestRate   | String        | APY, e.g. `0.01` represents `1%`                                                       |
| ts             | String        | Timestamp to calculated interest, Unix timestamp format in milliseconds, e.g. `1597026383085` |
