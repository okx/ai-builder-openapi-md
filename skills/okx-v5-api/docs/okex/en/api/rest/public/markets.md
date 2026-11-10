---
site: okex
lang: en
name: Get markets
method: GET
path: /api/v5/public/event-contract/markets
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get markets

Get markets for events in OKX prediction markets. Return data in expTime and floorStrike descending order.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/event-contract/markets`

> Request Example

```shell
GET /api/v5/public/event-contract/markets?seriesId=BTC-ABOVE-DAILY
```

#### Request Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| seriesId | String | Yes | Series ID, e.g. `BTC-ABOVE-DAILY` |
| eventId | String | No | Event ID, e.g. `BTC-ABOVE-DAILY-260224-1600` |
| instId | String | No | Instrument ID, e.g. `BTC-ABOVE-DAILY-260224-1600-65000` |
| state | String | No | Filter by market status.<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| limit | String | No | Number of results per request. Maximum is 100. Default is 100. |
| before | String | No | Pagination. Returns records newer than the requested `expTime`, not included. |
| after | String | No | Pagination. Returns records earlier than the requested `expTime`, not included. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "eventId": "BTC-ABOVE-DAILY-260224-1600",
            "instId": "BTC-ABOVE-DAILY-260224-1600-65000",
            "listTime": "1769697132335",
            "expTime": "1769697132335",
            "state": "live",
            "fixTime": "",
            "outcome": "0",
            "floorStrike": "120000",
            "capStrike": "",
            "settleValue": "",
            "disputed": false,
            "hitDir": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| seriesId | String | Series ID, e.g. `BTC-ABOVE-DAILY` |
| eventId | String | Event ID, e.g. `BTC-ABOVE-DAILY-260224-1600` |
| instId | String | Instrument ID, e.g. `BTC-ABOVE-DAILY-260224-1600-65000` |
| listTime | String | Listing time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| fixTime | String | Strike price fixing time, Unix timestamp format in milliseconds, e.g. `1597026383085`. Only applicable to `price_up_down` settlement method. |
| expTime | String | Strike time for this event, Unix timestamp format in milliseconds, e.g. `1597026383085`. Updated once the market is settled. |
| state | String | Market state.<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| disputed | Boolean | Whether the market has been disputed.<br>`true`<br>`false` |
| outcome | String | Market outcome.<br>`0`: Not available<br>`1`: YES<br>`2`: NO.<br>`1`/`2` only applicable when state is `expired` |
| floorStrike | String | Minimum expiration value that leads to a YES outcome |
| capStrike | String | Maximum expiration value that leads to a YES outcome for `between` method. `"INF"` indicates no upper bound (the topmost bracket).<br>Returns `""` for non-`between` methods. |
| settleValue | String | Settlement value<br>Only return when the state is `expired` |
| hitDir | String | Hit direction. Only applicable when the settlement method is `hit`.<br>`up`: price hit from below<br>`dn`: price hit from above<br>`""`: not applicable (non-`hit` methods) |
