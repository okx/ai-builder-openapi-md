---
site: my
lang: en
name: Get TVB performance summary
method: GET
path: /api/v5/affiliate/tvb/performance/summary
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get TVB performance summary

Retrieve an affiliate's aggregated TVB (Trading Volume Bonus) performance — trading volume, bonus amount, invitees and traders — for a specified time period. TVB is currently available in select regions only. Bonus accrues hourly and settles in `USDC`.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/tvb/performance/summary`

> Request Example

```shell
GET /api/v5/affiliate/tvb/performance/summary?periodType=last_30d
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| periodType    | String   | No           | Stats window for the metric fields.<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`this_week`<br>`today`<br>`total`<br>`custom`: pass `begin` and `end` to define a custom window.<br>The default is `total`. |
| begin         | String   | Conditional  | Custom stats-window start, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `end`. Inclusive. Used for a custom window — `periodType=custom`, or `periodType` omitted with both `begin` and `end` supplied. |
| end           | String   | Conditional  | Custom stats-window end, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `begin`. Inclusive. Used for a custom window — `periodType=custom`, or `periodType` omitted with both `begin` and `end` supplied. |

When `periodType=custom`, supply both `begin` and `end`. Supplying only one returns `50014`.<br>
For all other `periodType` values, server-defined windows are used and any `begin` / `end` passed alongside are ignored.<br>
When `periodType` is omitted, the window falls back to `custom` if both `begin` and `end` are present, otherwise `total`.<br>
A custom window (`end − begin`) must be between 1 and 90 days.<br>
`begin` must not be earlier than 180 days ago — data is available only for the past 180 days.<br>
`multiplier`, `ccy` and `uTime` are not window-scoped — they reflect the current config / data snapshot regardless of `periodType`.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "tradingVolBonus": "15.60",
            "validVol": "125000.00",
            "eligibleVol": "98400.00",
            "validInviteeCnt": "102",
            "validTraderCnt": "26",
            "eligibleTraderCnt": "19",
            "validDepAmt": "1420.113200000000000000",
            "validFirstTraderCnt": "17",
            "validFirstDepositorCnt": "12",
            "multiplier": "0.5",
            "ccy": "USDC",
            "uTime": "1777541513000"
        }
    ]
}
```

#### Response Parameters

| **Parameter**          | **Type** | **Description** |
| :--------------------- | :------- | :-------------- |
| tradingVolBonus        | String   | Accrued TVB bonus in the selected window, unit in `USDC`. Accrued value — includes amounts still pending/suspended. |
| validVol               | String   | Valid trading volume in the selected window — trading volume from valid invitees, unit in `USDC`. |
| eligibleVol            | String   | Eligible trading volume — the subset of valid volume that generates the bonus (bonus base), unit in `USDC`. |
| validInviteeCnt        | String   | Distinct valid invitees in the selected window. Valid = users with an active invite relationship to this affiliate. |
| validTraderCnt         | String   | Distinct valid invitees who traded in the selected window. |
| eligibleTraderCnt      | String   | Distinct traders who traded and generated bonus (subset of `validTraderCnt`). |
| validDepAmt            | String   | Deposit from valid invitees in the selected window, unit in `USDC`. |
| validFirstTraderCnt    | String   | Distinct valid invitees whose first-ever trade occurred within the selected window (FTT). Each invitee is counted at most once. |
| validFirstDepositorCnt | String   | Distinct valid invitees whose first-ever deposit occurred within the selected window (FTD). Each invitee is counted at most once. |
| multiplier             | String   | Affiliate's current bonus multiplier as a decimal ratio, e.g. `0.5` = 50%. Not window-scoped. |
| ccy                    | String   | Settlement currency. Constant `USDC`. |
| uTime                  | String   | Last data update timestamp, Unix timestamp in millisecond format. Returns `""` when no data is available yet. |
