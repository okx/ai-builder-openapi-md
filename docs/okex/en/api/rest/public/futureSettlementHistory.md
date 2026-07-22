---
site: okex
lang: en
name: Get futures settlement history
method: GET
path: /api/v5/public/settlement-history
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get futures settlement history

Retrieve settlement records of futures in the last 3 months.

#### Rate Limit: 40 requests per 2 seconds
#### Rate limit rule: IP + (Instrument Family)

#### HTTP Request

`GET /api/v5/public/settlement-history`

> Request Example

```shell
GET /api/v5/public/settlement-history?instFamily=XRP-USD
```

#### Request Parameters

| Parameter        | Type   | Required | Description                                                                        |
|:-----------------| :----- |:---------|:-----------------------------------------------------------------------------------|
| instFamily       | String | Yes      | Instrument family                                                                  |
| after            | String | No       | Pagination of data to return records earlier than (not include) the requested `ts` |
| before           | String | No       | Pagination of data to return records newer than (not include) the requested `ts`   |
| limit            | String | No       | Number of results per request. The maximum is `100`. The default is `100`          |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "instId": "XRP-USDT-250307",
                    "settlePx": "2.5192078615298715"
                }
            ],
            "ts": "1741161600000"
        },
        {
            "details": [
                {
                    "instId": "XRP-USDT-250307",
                    "settlePx": "2.5551316341327384"
                }
            ],
            "ts": "1741075200000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type             | Description                                                                  |
|:----------|:-----------------|:-----------------------------------------------------------------------------|
| ts        | String           | Settlement time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| details   | Array of objects | Settlement info                                                              |
| > instId  | String           | Instrument ID                                                                |
| > settlePx      | String           | Settlement price                                                             |
