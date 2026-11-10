---
site: okex
lang: en
name: GET / Lead trader weekly pnl
method: GET
path: /api/v5/copytrading/public-weekly-pnl
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Lead trader weekly pnl 

Public endpoint. Retrieve lead trader weekly pnl. Results are returned in counter chronological order.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-weekly-pnl`

> Request example

```shell
GET /api/v5/copytrading/public-weekly-pnl?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                                                                                                            |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                                                                                                                                                                          |
| uniqueCode  | String | Yes         | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                                       |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "beginTs": "1701014400000",
            "pnl": "-2.8428",
            "pnlRatio": "-0.0106"
        },
        {
            "beginTs": "1700409600000",
            "pnl": "81.8446",
            "pnlRatio": "0.3036"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**      | **Type** | **Description**                               |
|:-------------------| :------- |:----------------------------------------------|
| beginTs            | String | Begin time of pnl ratio on that week          |
| pnl          | String | Pnl on that week                              |
| pnlRatio          | String | Pnl ratio on that week                        |
