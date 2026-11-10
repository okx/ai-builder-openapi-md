---
site: my
lang: en
name: Get borrow interest and limit
method: GET
path: /api/v5/account/interest-limits
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get borrow interest and limit

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/interest-limits`

> Request Example

```shell
GET /api/v5/account/interest-limits?ccy=BTC

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get borrow interest and limit
result = accountAPI.get_interest_limits(
    ccy="BTC"
)
print(result)
```

#### Request Parameters

| Parameter     | Type   | Required | Description                                      |
| :--------- | :----- | :------- |:-------------------------------------------------|
| type | String | No       | Loan type<br>`2`: Market loans<br>Default is `2` |
| ccy | String | No      | Loan currency, e.g. `BTC`                        |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debt": "0.85893159114900247077000000000000",
            "interest": "0.00000000000000000000000000000000",
            "loanAlloc": "",
            "nextDiscountTime": "1729490400000",
            "nextInterestTime": "1729490400000",
            "records": [
                {
                    "availLoan": "",
                    "avgRate": "",
                    "ccy": "BTC",
                    "interest": "0",
                    "loanQuota": "175.00000000",
                    "posLoan": "",
                    "rate": "0.0000276",
                    "surplusLmt": "175.00000000",
                    "surplusLmtDetails": {},
                    "usedLmt": "0.00000000",
                    "usedLoan": "",
                    "interestFreeLiab": "",
                    "potentialBorrowingAmt": ""
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter                | Type             | Description                                                                                                                                                                                                                                  |
|:-------------------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| debt                     | String           | Current debt in `USD`                                                                                                                                                                                                                        |
| interest                 | String           | Current interest in `USD`, the unit is `USD`<br>Only applicable to `Market loans`                                                                                                                                                            |
| nextDiscountTime         | String           | Next deduct time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                |
| nextInterestTime         | String           | Next accrual time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                               |
| loanAlloc                | String           | VIP Loan allocation for the current trading account<br>1. The unit is percent(%). Range is [0, 100]. Precision is 0.01%<br>2. If master account did not assign anything, then "0"<br>3. "" if shared between master and sub-account	         |
| records                  | Array of objects | Details for currencies                                                                                                                                                                                                                       |
| > ccy                    | String           | Loan currency, e.g. `BTC`                                                                                                                                                                                                                    |
| > rate                   | String           | Current daily borrowing rate                                                                                                                                                                                                                           |
| > loanQuota              | String           | Borrow limit of master account<br>If loan allocation has been assigned, then it is the borrow limit of the current trading account                                                                                                           |
| > surplusLmt             | String           | Available amount across all sub-accounts<br>If loan allocation has been assigned, then it is the available amount to borrow by the current trading account 	                                                                                 |
| > usedLmt                | String           | Borrowed amount for current account<br>If loan allocation has been assigned, then it is the borrowed amount by the current trading account                                                                                                   |
| > interest               | String           | Interest to be deducted<br>Only applicable to `Market loans`                                                                                                                                                                                 |
| > interestFreeLiab               | String          | Interest-free liability for current account                                 |
| > potentialBorrowingAmt               | String          | Potential borrowing amount for current account                                 |
| > surplusLmtDetails      | Object           | <del>The details of available amount across all sub-accounts<br>The value of `surplusLmt` is the minimum value within this array. It can help you judge the reason that `surplusLmt` is not enough.<br>Only applicable to `VIP loans`</del>Deprecated	 |
| >> allAcctRemainingQuota | String           | <del>Total remaining quota for master account and sub-accounts 	 </del>Deprecated                                                                                                                                                                            |
| >> curAcctRemainingQuota | String           | <del>The remaining quota for the current account.<br>Only applicable to the case in which the sub-account is assigned the loan allocation</del>Deprecated	                                                                                                   |
| >> platRemainingQuota    | String           | <del>Remaining quota for the platform.<br>The format like  "600" will be returned when it is more than `curAcctRemainingQuota` or `allAcctRemainingQuota`</del>Deprecated	                                                                                   |
| > posLoan                | String           | <del>Frozen amount for current account (Within the locked quota) <br>Only applicable to `VIP loans`</del>Deprecated	                                                                                                                                         |
| > availLoan              | String           | <del>Available amount for current account (Within the locked quota)<br>Only applicable to `VIP loans`</del>Deprecated	                                                                                                                                       |
| > usedLoan               | String           | <del>Borrowed amount for current account<br>Only applicable to `VIP loans`</del>Deprecated	                                                                                                                                                                  |
| > avgRate                | String           | <del>Average hourly interest of borrowed coin<br>only applicable to `VIP loans`</del>Deprecated                                                                                                                                                      |
