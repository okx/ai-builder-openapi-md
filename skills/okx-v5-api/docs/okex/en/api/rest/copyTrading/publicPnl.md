---
site: okex
lang: en
name: GET / Lead trader daily pnl
method: GET
path: /api/v5/copytrading/public-pnl
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Lead trader daily pnl 

Public endpoint. Retrieve lead trader daily pnl. Results are returned in counter chronological order.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-pnl`

> Request example

```shell
GET /api/v5/copytrading/public-pnl?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD&lastDays=1

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                   |
| :------ | :----- |:---------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                 |
| uniqueCode  | String | Yes      | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters) |
| lastDays  | String | Yes      | Last days<br>`1`: last 7 days <br>`2`: last 30 days<br>`3`: last 90 days <br>`4`: last 365 days                                           |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "beginTs": "1701100800000",
            "pnl": "97.3309",
            "pnlRatio": "0.3672"
        },
        {
            "beginTs": "1701014400000",
            "pnl": "96.7755",
            "pnlRatio": "0.3651"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**      | **Type** | **Description**                     |
|:-------------------| :------- |:------------------------------------|
| beginTs            | String | Begin time on that day |
| pnl          | String | Accumulated pnl         |
| pnlRatio          | String | Accumulated pnl ratio  |
