---
site: okex
lang: en
name: Event contract markets channel
type: guide
---

### Event contract markets channel

Pushes event contract market status updates and floorStrike generation. No initial snapshot push.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
    "op": "subscribe",
    "args": [
        {
            "channel": "event-contract-markets",
            "instType": "EVENTS"
        }
    ]
}
```

#### Request Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| id | String | No | Unique identifier of the message. Provided by client. Returned in response. Alphanumeric, 1-32 characters. |
| op | String | Yes | Operation.<br>`subscribe`<br>`unsubscribe` |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name.<br>`event-contract-markets` |
| > instType | String | Yes | Instrument type.<br>`EVENTS` |

> Successful Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "event-contract-markets",
        "instType": "EVENTS"
    },
    "connId": "a4d3ae55"
}
```

> Failure Response Example

```json
{
    "id": "1512",
    "event": "error",
    "code": "60012",
    "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{\"channel\": \"event-contract-markets\", \"instType\": \"EVENTS\"}]}",
    "connId": "a4d3ae55"
}
```

#### Response Parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| id | String | Unique identifier of the message |
| event | String | Event.<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | Object | Subscribed channel |
| > channel | String | Channel name |
| > instType | String | Instrument type |
| code | String | Error code |
| msg | String | Error message |
| connId | String | WebSocket connection ID |

> Push Data Example

```json
{
    "arg": {
        "channel": "event-contract-markets"
    },
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "eventId": "BTC-ABOVE-DAILY-260224-1600",
            "instId": "BTC-ABOVE-DAILY-260224-1600-65000",
            "listTime": "1769697132335",
            "fixTime": "",
            "expTime": "1769697132335",
            "state": "live",
            "outcome": "0",
            "floorStrike": "120000",
            "capStrike": "",
            "settleValue": "",
            "disputed": false,
            "hitDir": ""
        }
    ]
}
```

#### Push Data Parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| arg | Object | Subscribed channel |
| > channel | String | Channel name |
| data | Array of objects | Subscribed data |
| > seriesId | String | Series ID, e.g. `BTC-ABOVE-DAILY` |
| > eventId | String | Event ID, e.g. `BTC-ABOVE-DAILY-260224-1600` |
| > instId | String | Instrument ID, e.g. `BTC-ABOVE-DAILY-260224-1600-65000` |
| > listTime | String | Listing time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > fixTime | String | Strike price fixing time, Unix timestamp format in milliseconds, e.g. `1597026383085`. Only applicable to `price_up_down` settlement method. |
| > expTime | String | Strike time for this event, Unix timestamp format in milliseconds, e.g. `1597026383085`. Updated once the market is settled. |
| > state | String | Market state.<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| > outcome | String | Market outcome.<br>`0`: Not available<br>`1`: YES<br>`2`: NO.<br>`1`/`2` only applicable when state is `expired` |
| > floorStrike | String | Minimum expiration value that leads to a YES outcome |
| > capStrike | String | Maximum expiration value that leads to a YES outcome for `between` method. `"INF"` indicates no upper bound (the topmost bracket).<br>Returns `""` for non-`between` methods. |
| > settleValue | String | Settlement value<br>Only return when the state is `expired` |
| > disputed | Boolean | Whether the market has been disputed.<br>`true`<br>`false` |
| > hitDir | String | Hit direction. Only applicable when the settlement method is `hit`.<br>`up`: price hit from below<br>`dn`: price hit from above<br>`""`: not applicable (non-`hit` methods) |
