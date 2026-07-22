---
site: okex
lang: en
name: Get interest rate and loan quota
method: GET
path: /api/v5/public/interest-rate-loan-quota
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

###  Get interest rate and loan quota

Retrieve interest rate

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/interest-rate-loan-quota`

> Request Example

```shell
GET /api/v5/public/interest-rate-loan-quota
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve interest rate and loan quota
result = publicDataAPI.get_interest_rate_loan_quota()
print(result)
```

> Response Example

```json
{
    "code": "0",
    "data": [
        {   
            "configCcyList": [
                {
                    "ccy": "USDT",
                    "rate": "0.00043728",
                }
            ],
            "basic": [
                {
                    "ccy": "USDT",
                    "quota": "500000",
                    "rate": "0.00043728"
                },
                {
                    "ccy": "BTC",
                    "quota": "10",
                    "rate": "0.00019992"
                }
            ],
            "vip": [
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "6",
                    "level": "VIP1"
                },
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "7",
                    "level": "VIP2"
                }
            ],
            "config": [
                {
                    "ccy": "USDT",
                    "stgyType": "0",    // normal
                    "quota": "xxxxxx",
                    "level": "VIP 8"
                },
                ......
                {
                    "ccy": "USDT",
                    "stgyType": "1",    // delta neutral
                    "quota": "xxxxx",
                    "level": "VIP 1"
                },
                ......
            ],
            "regular": [
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "1",
                    "level": "Lv1"
                },
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "2",
                    "level": "Lv1"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                                            |
| :----- | :----- |:-------------------------------------------------------|
| basic      | Array of objects  | Basic interest rate                                    |
| > ccy      | String | Currency                                               |
| > rate     | String | Daily borrowing rate                                             |
| > quota    | String | Max borrow                                             |
| vip        | Array of objects  | Interest info for vip users                            |
| > level    | String | VIP Level, e.g. `VIP1`                                 |
| > loanQuotaCoef     | String | Loan quota coefficient. Loan quota = `quota` * `level` |
| > irDiscount    | String | <del>Interest rate discount</del>(Deprecated)          |
| regular         | Array of objects  | Interest info for regular users                        |
| > level         | String | Regular user Level, e.g. `Lv1`                         |
| > loanQuotaCoef | String | Loan quota coefficient. Loan quota = `quota` * `level` |
| > irDiscount    | String | <del>Interest rate discount</del>(Deprecated)          |
| configCcyList | Array of strings | Currencies that have loan quota configured using customized absolute value.<br>Users should refer to config to get the loan quota of a currency which is listed in configCcyList, instead of getting it from basic/vip/regular. |
| > ccy | String | Currency |
| > rate | String | Daily rate |
| config | Array of objects | The currency details of loan quota configured using customized absolute value |
| > ccy | String | Currency |
| > stgyType | String | Strategy type<br>`0`: general strategy<br>`1`: delta neutral strategy<br>If only `0` is returned for a currency, it means the loan quota is shared between accounts in general strategy and accounts in delta neutral strategy; if both `0/1` are returned for a currency, it means accounts in delta neutral strategy have separate loan quotas. |
| > quota | String | Loan quota in absolute value |
| > level | String | VIP level |
