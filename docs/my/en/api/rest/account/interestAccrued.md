---
site: my
lang: en
name: Get interest accrued data
method: GET
path: /api/v5/account/interest-accrued
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get interest accrued data

Get the interest accrued data for the past year

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/interest-accrued`

> Request Example

```shell
GET /api/v5/account/interest-accrued

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get interest accrued data
result = accountAPI.get_interest_accrued()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                            |
|:----------| :----- |:---------|:---------------------------------------------------------------------------------------------------------------------------------------|
| type      | String | No       | Loan type<br>`2`: Market loans<br>Default is `2`                                                                                       |
| ccy       | String | No       | Loan currency, e.g. `BTC`<br>Only applicable to `Market loans`<br>Only applicable to`MARGIN`                                           |
| instId    | String | No       | Instrument ID, e.g. `BTC-USDT`<br>Only applicable to `Market loans`                                                                    |
| mgnMode   | String | No       | Margin mode<br>`cross`  <br>`isolated`<br>Only applicable to `Market loans`                                                            |
| after     | String | No       | Pagination of data to return records earlier than the requested timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| before    | String | No       | Pagination of data to return records newer than the requested, Unix timestamp format in milliseconds, e.g. `1597026383085`             |
| limit     | String | No       | Number of results per request. The maximum is `100`. The default is `100`.                                                             |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "instId": "",
            "interest": "0.0003960833333334",
            "interestRate": "0.0000040833333333",
            "liab": "97",
            "totalLiab": "",
            "interestFreeLiab": "",
            "mgnMode": "",
            "ts": "1637312400000",
            "type": "1"
        },
        {
            "ccy": "USDT",
            "instId": "",
            "interest": "0.0004083333333334",
            "interestRate": "0.0000040833333333",
            "liab": "100",
            "mgnMode": "",
            "ts": "1637049600000",
            "type": "1"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter    | Type   | Description                                                                                 |
|:-------------| :----- |:--------------------------------------------------------------------------------------------|
| type         | String | Loan type<br>`2`: Market loans                                                              |
| ccy          | String | Loan currency, e.g. `BTC`                                                                   |
| instId       | String | Instrument ID, e.g. `BTC-USDT`<br>Only applicable to `Market loans`                         |
| mgnMode      | String | Margin mode<br>`cross`  <br>`isolated`                                                      |
| interest     | String | Interest accrued                                                                                   |
| interestRate | String | Hourly borrowing interest rate                                                                     |
| liab         | String | Liability                                                                                   |
| totalLiab               | String          | Total liability for current account                                 |
| interestFreeLiab               | String          | Interest-free liability for current account                                 |
| ts           | String | Timestamp for interest accrued, Unix timestamp format in milliseconds, e.g. `1597026383085` |
