---
site: okex
lang: en
name: GET / Get GLP today performance
method: GET
path: /api/v5/users/glp/todayperformance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get GLP today performance

Retrieve the current day and month-to-date (MTD) GLP performance snapshot for the authenticated account, covering all enrolled programs (Spot / Perp / Expiry & Nitro). No request parameters; the account is resolved from the API key. Only enrolled GLP market maker accounts can access this endpoint; sub-accounts resolve to their master account.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/users/glp/todayperformance`

> Request Example

```shell
GET /api/v5/users/glp/todayperformance
```

#### Request Parameters

None. The account is resolved from the authenticated session.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "dataReady": true,
            "dataDate": "2026-07-13",
            "account": {
                "masterAccountId": "832545488879789797",
                "combinedAccountIds": ["832545488879789798"]
            },
            "programs": [
                {
                    "program": "SPOT",
                    "marketMakerBusinessId": "1",
                    "enrollmentStatus": "ENROLLED",
                    "marketMakerLevelId": "42",
                    "enrolledTierDisplay": "Tier 1 Class A",
                    "qualifyingPool": "TYPE_A",
                    "qualifyingRows": ["TOTAL"],
                    "daily": {
                        "volume": {
                            "typeA": {"maker": "1000000.00", "taker": "1000000.00"},
                            "typeBTotal": {"maker": "1000000.00", "taker": "1000000.00"},
                            "tradfiX2": {"maker": "1000000.00", "taker": "1000000.00"},
                            "total": {"maker": "2000000.00", "taker": "2000000.00"}
                        },
                        "share": {
                            "typeA": {"maker": "0.0000", "taker": "0.0000"},
                            "typeBAdj": {"maker": "0.0000", "taker": "0.0000"},
                            "total": {"maker": "0.0000", "taker": "0.0000"}
                        }
                    },
                    "mtd": {
                        "volume": {
                            "typeA": {"maker": "30000000.00", "taker": "30000000.00"},
                            "typeBTotal": {"maker": "30000000.00", "taker": "30000000.00"},
                            "tradfiX2": {"maker": "30000000.00", "taker": "30000000.00"},
                            "total": {"maker": "60000000.00", "taker": "60000000.00"}
                        },
                        "share": {
                            "typeA": {"maker": "0.0000", "taker": "0.0000"},
                            "typeBAdj": {"maker": "0.0000", "taker": "0.0000"},
                            "total": {"maker": "0.0000", "taker": "0.0000"}
                        },
                        "mtdStatus": "QUALIFIED",
                        "qualifyingShare": {"maker": "0.0000", "taker": "0.0000"}
                    }
                }
            ]
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| dataReady     | Boolean  | Whether data is available for the `dataDate`. When `false`, `programs` is an empty array |
| dataDate      | String   | Data snapshot date in `yyyy-MM-dd` format (UTC+8). Typically T-1; falls back to T-2 when T-1 computation is incomplete |
| account       | Object   | Account identity block |
| > masterAccountId | String | Master account ID |
| > combinedAccountIds | Array of strings | Sibling account IDs in the same institutional group (excludes self). Empty array if no group |
| programs      | Array of objects | Performance data for each enrolled GLP program. Empty when `dataReady` is `false` |
| > program     | String   | GLP business line identifier.<br>`SPOT`<br>`PERP`<br>`FUT_NTO`: Expiry & Nitro |
| > marketMakerBusinessId | String | Market maker business ID for this program |
| > enrollmentStatus | String | Enrollment status. Currently always `ENROLLED` |
| > marketMakerLevelId | String | Current tier level ID |
| > enrolledTierDisplay | String | Current tier display name, e.g. `Tier 1 Class A` |
| > qualifyingPool | String | Pool that determines current tier.<br>`TYPE_A`<br>`TYPE_B_ADJ`<br>`TYPE_A_AND_B` |
| > qualifyingRows | Array of strings | Keys of qualifying rows for UI highlighting, e.g. `["TOTAL"]` |
| > daily       | Object   | Today's performance metrics. Contains `volume` and `share` (see structure below) |
| > mtd         | Object   | Month-to-date performance metrics. Contains `volume`, `share` (same structure as `daily`), plus the additional fields below |
| >> mtdStatus  | String   | MTD qualification status.<br>`QUALIFIED`<br>`UPGRADE`<br>`DOWNGRADE` |
| >> qualifyingShare | Object | Share in the qualifying pool. Contains `maker` (String) and `taker` (String) |

**Volume and share structure**

Both `daily` and `mtd` contain a `volume` Object and a `share` Object. Each Object holds category keys, and each category is itself an Object with `maker` (String) and `taker` (String) fields.

| **Category** | **In `volume`** | **In `share`** | **Description** |
| :----------- | :-------------- | :------------- | :-------------- |
| typeA        | Yes             | Yes            | Type A. `null` for `FUT_NTO` |
| typeBTotal   | Yes             | No             | Type B total. `null` for `FUT_NTO` |
| typeBAdj     | No              | Yes            | Type B adjusted. `null` for `FUT_NTO` |
| tradfiX2     | Yes             | No             | TradFi volume doubled. `null` for `FUT_NTO` |
| total        | Yes             | Yes            | Total across all types. Always present |

- **`volume`** values: USD notional as String with 2 decimal places (e.g. `"1000000.00"`)
- **`share`** values: decimal string with 4 decimal places, no `%` suffix (e.g. `"0.0012"`)
