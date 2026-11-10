---
site: turkey
lang: en
name: GET / Easy convert history
method: GET
path: /api/v5/trade/easy-convert-history
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Easy convert history 

Get the history and status of easy convert trades in the past 7 days.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/easy-convert-history`

> Request Example

```shell
GET /api/v5/trade/easy-convert-history
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Get the history of easy convert trades
result = tradeAPI.get_easy_convert_history()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                        |
| :------- | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------|
| after    | String | No       | Pagination of data to return records earlier than the requested time (exclude), Unix timestamp format in milliseconds, e.g. `1597026383085`          |
| before   | String | No       | Pagination of data to return records newer than the requested time (exclude), Unix timestamp format in milliseconds, e.g. `1597026383085`            |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                             |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fillFromSz": "0.1761712511667539",
            "fillToSz": "6.7342205900000000",
            "fromCcy": "OKB",
            "status": "filled",
            "toCcy": "ADA",
            "acct": "18",
            "uTime": "1661313307979"
        },
        {
            "fillFromSz": "0.1722106121112177",
            "fillToSz": "2.9971018300000000",
            "fromCcy": "OKB",
            "status": "filled",
            "toCcy": "USDC",
            "acct": "18",
            "uTime": "1661313307979"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type      | Description                                                                                     |
|:-----------|:----------|:------------------------------------------------------------------------------------------------|
| fromCcy    | String    | Type of small payment currency convert from                                                     |
| fillFromSz | String    | Amount of small payment currency convert from                                                   |
| toCcy      | String    | Type of mainstream currency convert to                                                          |
| fillToSz   | String    | Amount of mainstream currency convert to                                                        |
| acct       | String    | The account where the mainstream currency is located<br>`6`: Funding account <br>`18`: Trading account    |
| status     | String    | Current status of easy convert <br>`running`: Running <br>`filled`: Filled <br>`failed`: Failed |
| uTime      | String    | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`                         |
