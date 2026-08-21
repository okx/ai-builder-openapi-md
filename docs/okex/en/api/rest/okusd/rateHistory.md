---
site: okex
lang: en
name: GET / Get APR History
method: GET
path: /api/v5/finance/okusd/rate/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Get APR History

Retrieve the historical APR snapshots for OKUSD. Results are returned in descending order by timestamp (newest first). This endpoint requires API Key authentication even though the data is product-level and not user-specific.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/okusd/rate/history`

> Request Example

```shell
GET /api/v5/finance/okusd/rate/history?limit=10
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| limit | String | No | Number of results per page. Default `"30"`. Maximum `"100"` |
| begin | String | No | Start time filter (`ts`, Unix ms, inclusive). Maximum query span is 6 months |
| end | String | No | End time filter (`ts`, Unix ms, inclusive). Defaults to the current time if not provided |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        { "apr": "0.0418", "ts": "1718500000000" },
        { "apr": "0.0395", "ts": "1718413600000" }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| apr | String | OKUSD APR at this snapshot, e.g. `"0.0418"` represents 4.18% |
| ts | String | Snapshot timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
