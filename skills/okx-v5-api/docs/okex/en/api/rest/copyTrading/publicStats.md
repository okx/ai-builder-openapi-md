---
site: okex
lang: en
name: GET / Lead trader stats
method: GET
path: /api/v5/copytrading/public-stats
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Lead trader stats 

Public endpoint. Key data related to lead trader performance.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-stats`

> Request example

```shell
GET /api/v5/copytrading/public-stats?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD&lastDays=1

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                                                                                                            |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                                                                                                                                                                          |
| uniqueCode  | String | Yes         | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                                       |
| lastDays  | String | Yes      | Last days<br>`1`: last 7 days <br>`2`: last 30 days<br>`3`: last 90 days <br>`4`: last 365 days                                           |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "avgSubPosNotional": "213.1038",
            "ccy": "USDT",
            "curCopyTraderPnl": "96.8071",
            "investAmt": "265.095252476476294",
            "lossDays": "1",
            "profitDays": "2",
            "winRatio": "0.6667"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**      | **Type** | **Description**                       |
|:-------------------| :------- |:--------------------------------------|
| winRatio            | String | Win ratio                             |
| profitDays          | String | Profit days                           |
| lossDays              | String  | Loss days                             |
| curCopyTraderPnl              | String  | Current copy trader pnl (USDT)        |
| avgSubPosNotional              | String  | Average lead position notional (USDT) |
| investAmt              | String  | Investment amount (USDT)              |
| ccy              | String  | Margin currency                       |
