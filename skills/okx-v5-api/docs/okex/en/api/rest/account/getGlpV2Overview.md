---
site: okex
lang: en
name: GET / Get GLP v2 overview
method: GET
path: /api/v5/users/glp/v2/overview
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Get GLP v2 overview

Retrieve active GLP enrollments, current tiers, estimated tiers, tier requirements, and week-to-date performance for the authenticated account. A sub-account API key retrieves the master account's GLP data.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### Permission: Read

#### HTTP Request

`GET /api/v5/users/glp/v2/overview`

> Request Example

```shell
GET /api/v5/users/glp/v2/overview
```

#### Request Parameters

No request parameters.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "programs": [
                {
                    "bizId": "16",
                    "program": "GLP_B",
                    "side": "MAKER",
                    "instrument": "PERP",
                    "currentTier": "Tier 2",
                    "estNextTier": "Tier 2",
                    "estNextStatus": "qualify",
                    "gracePeriodEndTime": "",
                    "requirement": [
                        {
                            "tier": "Tier 1",
                            "share": "0.005"
                        },
                        {
                            "tier": "Tier 2",
                            "share": "0.003"
                        }
                    ],
                    "performance": [
                        {
                            "date": "1726531200000",
                            "assessPeriod": "PREVIOUS",
                            "share": "0.0035",
                            "vol": "140000"
                        },
                        {
                            "date": "1726876800000",
                            "assessPeriod": "CURRENT",
                            "share": "0.004",
                            "vol": "200"
                        }
                    ]
                }
            ]
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| programs | Array of objects | Active GLP enrollments. Empty array when none available. |
| > bizId | String | Product and trading-side identifier.<br>`14`: SPOT MAKER<br>`15`: FUT_NTO MAKER<br>`16`: PERP MAKER<br>`17`: SPOT TAKER<br>`18`: FUT_NTO TAKER<br>`19`: PERP TAKER |
| > program | String | GLP program name.<br>`GLP_A`<br>`GLP_B` |
| > side | String | Trading side.<br>`MAKER`<br>`TAKER` |
| > instrument | String | GLP product category.<br>`SPOT`: Spot trading<br>`PERP`: Perpetuals<br>`FUT_NTO`: Expiry Futures and Nitro |
| > currentTier | String | GLP tier currently in effect. Empty string when unavailable. |
| > estNextTier | String | Estimated GLP tier based on latest assessment. Empty string when unavailable. |
| > estNextStatus | String | Estimated tier outcome.<br>`disqualify`<br>`downgrade`<br>`qualify`<br>`upgrade`<br>Empty string when unavailable. |
| > gracePeriodEndTime | String | Earliest date on which a downgrade can take effect. Unix timestamp in milliseconds. Empty string when not applicable. |
| > requirement | Array of objects | Market-share thresholds for GLP tiers. |
| >> tier | String | GLP tier name. |
| >> share | String | Required market share as a decimal fraction. `"0.007"` = 0.7%. `null` when unavailable. |
| > performance | Array of objects | Week-to-date performance by trading date. |
| >> date | String | Trading date. Unix timestamp in milliseconds. |
| >> assessPeriod | String | Assessment period containing this trading date.<br>`PREVIOUS`<br>`CURRENT` |
| >> share | String | Week-to-date market share as a decimal fraction. `null` when unavailable. |
| >> vol | String | Week-to-date trading volume. `null` when unavailable. |
