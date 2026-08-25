---
site: okex
lang: en
name: Get events
method: GET
path: /api/v5/public/event-contract/events
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get events

Get events for a series in OKX prediction markets. Returns all event records, including expired ones. Return data in expTime and eventId descending order.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/event-contract/events`

> Request Example

```shell
GET /api/v5/public/event-contract/events?seriesId=BTC-ABOVE-DAILY
```

#### Request Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| seriesId | String | Yes | Series ID, e.g. `BTC-ABOVE-DAILY` |
| eventId | String | No | Event ID, e.g. `BTC-ABOVE-DAILY-260224-1600` |
| state | String | No | Event state filter.<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
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
            "expTime": "1769697132335",
            "state": "live"
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
| fixTime | String | Strike price fixing time, Unix timestamp format in milliseconds, e.g. `1597026383085`. Only applicable to `price_up_down` settlement method. |
| expTime | String | The specific strike time this event is based on, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| state | String | Event state.<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
