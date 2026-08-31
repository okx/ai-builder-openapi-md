---
site: okex
lang: en
name: GET / Lead trader currency preferences
method: GET
path: /api/v5/copytrading/public-preference-currency
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Lead trader currency preferences 

Public endpoint. The most frequently traded crypto of this lead trader. Results are sorted by ratio from large to small.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-preference-currency`

> Request example

```shell
GET /api/v5/copytrading/public-preference-currency?instType=SWAP&uniqueCode=CB4594A3BB5D3538

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
            "ccy": "ETH",
            "ratio": "0.8881"
        },
        {
            "ccy": "BTC",
            "ratio": "0.0666"
        },
        {
            "ccy": "YFII",
            "ratio": "0.0453"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**      | **Type** | **Description**           |
|:-------------------| :------- |:--------------------------|
| ccy            | String | Currency                  |
| ratio          | String | Ratio. 0.1 represents 10% |
