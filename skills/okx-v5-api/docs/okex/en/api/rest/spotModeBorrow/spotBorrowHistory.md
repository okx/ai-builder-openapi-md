---
site: okex
lang: en
name: Get borrow/repay history
method: GET
path: /api/v5/account/spot-borrow-repay-history
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get borrow/repay history

Retrieve the borrow/repay history under `Spot mode`

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/spot-borrow-repay-history`

> Request Example

```shell
GET /api/v5/account/spot-borrow-repay-history
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"   # Production trading:0 , demo trading:1
accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.spot_borrow_repay_history(ccy="USDT", type="auto_borrow")
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                  |
|:----------| :----- |:---------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| ccy       | String | No       | Currency, e.g. `BTC`                                                                                                                         |
| type      | String | No       | Event type<br>`auto_borrow`<br>`auto_repay`<br>`manual_borrow`<br>`manual_repay`                                                             |
| after     | String | No       | Pagination of data to return records earlier than the requested `ts` (included), Unix timestamp format in milliseconds, e.g. `1597026383085` |
| before    | String | No       | Pagination of data to return records newer than the requested `ts`(included), Unix timestamp format in milliseconds, e.g. `1597026383085`    |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                       |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accBorrowed": "0",
            "amt": "6764.802661157592",
            "ccy": "USDT",
            "ts": "1725330976644",
            "type": "auto_repay"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter   | Type   | Description                                                                          |
|:------------| :----- |:-------------------------------------------------------------------------------------|
| ccy         | String | Currency, e.g. `BTC`                                                                 |
| type        | String | Event type<br>`auto_borrow`<br>`auto_repay`<br>`manual_borrow`<br>`manual_repay`     |
| amt         | String | Amount                                                                               |
| accBorrowed | String | Accumulated borrow amount                                                            |
| ts          | String | Timestamp for the event, Unix timestamp format in milliseconds, e.g. `1597026383085` |
