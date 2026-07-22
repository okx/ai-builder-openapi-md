---
site: my
lang: en
name: Get performance summary
method: GET
path: /api/v5/affiliate/performance/summary
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get performance summary

Aggregated affiliate performance metrics for a specified time period.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/performance/summary`

> Request Example

```shell
GET /api/v5/affiliate/performance/summary?periodType=total
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| periodType    | String   | No           | Stats window for all summary fields except `uTime`.<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>`custom`: pass `begin` and `end` to define a custom window.<br>The default is `total`. |
| begin         | String   | Conditional  | Custom stats-window start, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `end`. Inclusive. |
| end           | String   | Conditional  | Custom stats-window end, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `begin`. Inclusive. |

When `periodType=custom`, supply both `begin` and `end`. Supplying only one returns `50014`.<br>
For all other `periodType` values, server-defined windows are used and any `begin` / `end` passed alongside are ignored.<br>
`periodType` / `begin` / `end` filter the statistics window for all summary fields (`inviteeCnt`, `depAmt`, `details.*`). `uTime` always reflects the latest data snapshot regardless of the window.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "uTime": "1777541513000",
            "inviteeCnt": "102",
            "depAmt": "1756.287846940199989393",
            "details": [
                {
                    "commissionCategory": "SPOT",
                    "firstTraderCnt": "17",
                    "traderCnt": "17",
                    "vol": "21548.6417826825604",
                    "commission": "3.322319946747010328"
                },
                {
                    "commissionCategory": "DERIVATIVE",
                    "firstTraderCnt": "9",
                    "traderCnt": "9",
                    "vol": "94531.94802",
                    "commission": "3.25142568535855"
                },
                {
                    "commissionCategory": "BSC",
                    "firstTraderCnt": "0",
                    "traderCnt": "0",
                    "vol": "0",
                    "commission": "0"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| **Parameter**        | **Type**         | **Description** |
| :------------------- | :--------------- | :-------------- |
| uTime                | String           | Last data update timestamp, Unix timestamp in millisecond format. |
| inviteeCnt           | String           | Total number of invitees. |
| depAmt               | String           | Total deposit amount, unit in `USDT`. |
| details              | Array of objects | One entry per instrument bucket. |
| > commissionCategory | String           | Commission calculation category.<br>`SPOT`<br>`DERIVATIVE`<br>`BSC` |
| > firstTraderCnt     | String           | Count of invitees whose lifetime-first trade occurred within the selected window, scoped to this `commissionCategory`. Each invitee can contribute at most once in their lifetime. |
| > traderCnt          | String           | Number of invitees who traded in this `commissionCategory` bucket within the selected window. Period-scoped. |
| > vol                | String           | Trading volume in this `commissionCategory` bucket within the selected window, unit in `USDT`. Period-scoped — distinct from `totalVol` on `/invitee/list` and `/sub-affiliate/list` which is lifetime cumulative. |
| > commission         | String           | Commission earned in this `commissionCategory` bucket within the selected window, unit in `USDT`. Period-scoped — distinct from `totalCommission` on other endpoints which is lifetime cumulative. |
