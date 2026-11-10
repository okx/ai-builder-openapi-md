---
site: okex
lang: en
name: GET / Get GLP v2 assessment details
method: GET
path: /api/v5/users/glp/v2/details
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Get GLP v2 assessment details

Retrieve daily and week-to-date performance for the selected GLP product and trading side within the current assessment period. A sub-account API key retrieves the master account's GLP data.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### Permission: Read

#### HTTP Request

`GET /api/v5/users/glp/v2/details`

> Request Example

```shell
GET /api/v5/users/glp/v2/details?bizId=16
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| bizId | String | Yes | Product and trading-side identifier.<br>`14`: SPOT MAKER<br>`15`: FUT_NTO MAKER<br>`16`: PERP MAKER<br>`17`: SPOT TAKER<br>`18`: FUT_NTO TAKER<br>`19`: PERP TAKER |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "assessStartTime": "1726790400000",
            "assessEndTime": "1727308800000",
            "performance": [
                {
                    "date": "1726790400000",
                    "origVol": "100",
                    "origShare": "0.004",
                    "adjVol": "200",
                    "adjShare": "0.004",
                    "accOrigVol": "100",
                    "accOrigShare": "0.004",
                    "accAdjVol": "200",
                    "accAdjShare": "0.004"
                },
                {
                    "date": "1726876800000",
                    "origVol": "200",
                    "origShare": "0.004",
                    "adjVol": "400",
                    "adjShare": "0.004",
                    "accOrigVol": "300",
                    "accOrigShare": "0.004",
                    "accAdjVol": "600",
                    "accAdjShare": "0.004"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| assessStartTime | String | Start of the assessment period. Unix timestamp in milliseconds. |
| assessEndTime | String | End of the assessment period. Unix timestamp in milliseconds. |
| performance | Array of objects | Daily and week-to-date performance. Empty array when no data available. |
| > date | String | Trading date. Unix timestamp in milliseconds. |
| > origVol | String | Daily trading volume before weighting. `null` when unavailable. |
| > origShare | String | Daily market share as a decimal fraction. `null` when unavailable. |
| > adjVol | String | Adjusted daily trading volume. `null` when unavailable. |
| > adjShare | String | Adjusted daily market share as a decimal fraction. `null` when unavailable. |
| > accOrigVol | String | Week-to-date trading volume before weighting. `null` when unavailable. |
| > accOrigShare | String | Week-to-date market share as a decimal fraction. `null` when unavailable. |
| > accAdjVol | String | Adjusted week-to-date trading volume. `null` when unavailable. |
| > accAdjShare | String | Adjusted week-to-date market share as a decimal fraction. `null` when unavailable. |
