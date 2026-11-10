---
site: okex
lang: en
name: GET / Get GLP v2 performance history
method: GET
path: /api/v5/users/glp/v2/history/performance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Get GLP v2 performance history

Retrieve weekly assessment results, tier outcomes, and performance metrics for the selected GLP product and trading side. A sub-account API key retrieves the master account's GLP data.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### Permission: Read

#### HTTP Request

`GET /api/v5/users/glp/v2/history/performance`

> Request Example

```shell
GET /api/v5/users/glp/v2/history/performance?bizId=16
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
            "performance": [
                {
                    "assessStartTime": "1726185600000",
                    "assessEndTime": "1726704000000",
                    "tier": "Tier 3",
                    "nextTier": "Tier 2",
                    "accOrigVol": "100000",
                    "accOrigShare": "0.004",
                    "accAdjVol": "200000",
                    "accAdjShare": "0.004",
                    "assessResult": "upgrade"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| performance | Array of objects | Weekly assessment results. Empty array when no history available. |
| > assessStartTime | String | Start of the assessment period. Unix timestamp in milliseconds. |
| > assessEndTime | String | End of the assessment period. Unix timestamp in milliseconds. |
| > tier | String | GLP tier used for this assessment. |
| > nextTier | String | Tier determined by this assessment. Empty string when unavailable. |
| > accOrigVol | String | Trading volume for the assessment period before weighting. `null` when unavailable. |
| > accOrigShare | String | Market share for the assessment period as a decimal fraction. `null` when unavailable. |
| > accAdjVol | String | Adjusted trading volume for the assessment period. `null` when unavailable. |
| > accAdjShare | String | Adjusted market share for the assessment period as a decimal fraction. `null` when unavailable. |
| > assessResult | String | Tier outcome.<br>`disqualify`<br>`downgrade`<br>`qualify`<br>`upgrade`<br>Empty string when unavailable. |
