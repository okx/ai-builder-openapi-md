---
site: okex
lang: en
name: GET / Get GLP historical performance
method: GET
path: /api/v5/users/glp/historicalperformance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get GLP historical performance

Retrieve daily GLP performance history for a single program. Results are sorted newest-first (descending by date). Only enrolled GLP market maker accounts can access this endpoint; sub-accounts resolve to their master account.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/users/glp/historicalperformance`

> Request Example

```shell
GET /api/v5/users/glp/historicalperformance?program=SPOT
GET /api/v5/users/glp/historicalperformance?program=SPOT&begin=1751299200000&end=1753804800000&limit=31
```

#### Request Parameters

| Parameter | Type   | Required | Description |
| :-------- | :----- | :------- | :---------- |
| program   | String | Yes      | GLP business line identifier.<br>`SPOT`<br>`PERP`<br>`FUT_NTO` |
| begin     | String | No       | Begin date filter (inclusive). Unix ms String, e.g. `"1751299200000"`. Default: first day of current month (UTC+8) |
| end       | String | No       | End date filter (inclusive). Unix ms String. Default: today (UTC+8) |
| limit     | String | No       | Max records per page. Default `"31"`, max `"100"` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "date": "2026-07-13",
            "volume": {
                "typeA": {"maker": "1000000.00", "taker": "1000000.00"},
                "typeBTotal": {"maker": "1000000.00", "taker": "1000000.00"},
                "tradfiX2": {"maker": "1000000.00", "taker": "1000000.00"},
                "total": {"maker": "2000000.00", "taker": "2000000.00"}
            },
            "share": {
                "typeA": {"maker": "0.0012", "taker": "0.0010"},
                "typeBAdj": {"maker": "0.0008", "taker": "0.0007"},
                "total": {"maker": "0.0010", "taker": "0.0009"}
            }
        },
        {
            "date": "2026-07-12",
            "volume": {
                "typeA": {"maker": "950000.00", "taker": "980000.00"},
                "typeBTotal": {"maker": "850000.00", "taker": "900000.00"},
                "tradfiX2": {"maker": "800000.00", "taker": "820000.00"},
                "total": {"maker": "1800000.00", "taker": "1880000.00"}
            },
            "share": {
                "typeA": {"maker": "0.0011", "taker": "0.0009"},
                "typeBAdj": {"maker": "0.0007", "taker": "0.0006"},
                "total": {"maker": "0.0009", "taker": "0.0008"}
            }
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| date          | String   | Date in `yyyy-MM-dd` format (UTC+8) |
| volume        | Object   | Trading volume by pool type (USD notional, 2 decimal places). Same structure as the today performance endpoint's `daily.volume` |
| share         | Object   | Market share by pool type (decimal string, 4 decimal places, no `%` suffix). Same structure as the today performance endpoint's `daily.share` |

**Error Codes**

| Error code | HTTP status code | Error message |
| :--------- | :--------------- | :------------ |
| 50030      | 200              | You don't have permission to use this API endpoint |
| 50014      | 200              | Parameter {param0} can not be empty |
| 51000      | 200              | Parameter error |
| 50016      | 200              | Parameter {param0} does not match parameter {param1} |
