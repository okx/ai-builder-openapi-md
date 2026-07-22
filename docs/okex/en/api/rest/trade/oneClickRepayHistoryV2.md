---
site: okex
lang: en
name: GET / One-click repay history (New)
method: GET
path: /api/v5/trade/one-click-repay-history-v2
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / One-click repay history (New)

Get the history and status of one-click repay trades in the past 7 days. Only applicable to `SPOT mode`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/one-click-repay-history-v2`

> Request Example

```shell
GET /api/v5/trade/one-click-repay-history-v2
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)
result = tradeAPI.oneclick_repay_history_v2()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                      |
| :------- | :----- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------|
| after    | String | No       | Pagination of data to return records earlier than (included) the requested time `ts` , Unix timestamp format in milliseconds, e.g. `1597026383085` |
| before   | String | No       | Pagination of data to return records newer than (included) the requested time `ts`, Unix timestamp format in milliseconds, e.g. `1597026383085`    |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                           |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "fillDebtSz": "9.079631989",
            "ordIdInfo": [
                {
                    "cTime": "1742194485439",
                    "fillPx": "1",
                    "fillSz": "9.088651",
                    "instId": "USDC-USDT",
                    "ordId": "2338478342062235648",
                    "ordType": "ioc",
                    "px": "1.0049",
                    "side": "buy",
                    "state": "filled",
                    "sz": "9.0886514537313433"
                },
                {
                    "cTime": "1742194482326",
                    "fillPx": "83271.9",
                    "fillSz": "0.00010969",
                    "instId": "BTC-USDT",
                    "ordId": "2338478237607288832",
                    "ordType": "ioc",
                    "px": "82856.7",
                    "side": "sell",
                    "state": "filled",
                    "sz": "0.000109696512171"
                }
            ],
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "status": "filled",
            "ts": "1742194481852"
        },
        {
            "debtCcy": "USDC",
            "fillDebtSz": "100",
            "ordIdInfo": [],
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "status": "filled",
            "ts": "1742192217511"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter    | Type            | Description                                                                                        |
|:-------------|:----------------|:---------------------------------------------------------------------------------------------------|
| debtCcy      | String          | Debt currency                                                                                      |
| repayCcyList | Array of strings          | Repay currency list, e.g. ["USDC","BTC"]                                                           |
| fillDebtSz   | String          | Amount of debt currency transacted                                                                 |
| status       | String          | Current status of one-click repay <br>`running`: Running <br>`filled`: Filled <br>`failed`: Failed |
| ordIdInfo    | Array of objects | Order info                                                                                         |
| > ordId      | String          | Order ID                                                                                           |
| > instId     | String          | Instrument ID, e.g. `BTC-USDT`                                                                     |
| > ordType    | String          | Order type<br>`ioc`: Immediate-or-cancel order                                                                              |
| > side       | String          | Side<br>`buy`<br>`sell`                                                                            |
| > px         | String          | Price                                                                                              |
| > sz         | String          | Quantity to buy or sell                                                                            |
| > fillPx     | String          | Last filled price.<br>If none is filled, it will return "".                                        |
| > fillSz     | String          | Last filled quantity                                                                               |
| > state      | String          | State<br>`filled`<br>`canceled`                                                                    |
| > cTime      | String          | Creation time for order, Unix timestamp format in milliseconds, e.g. `1597026383085`               |
| ts           | String          | Request time, Unix timestamp format in milliseconds, e.g. `1597026383085`                          |
