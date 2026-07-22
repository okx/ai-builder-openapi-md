---
site: okex
lang: en
name: Get instrument tick bands
method: GET
path: /api/v5/public/instrument-tick-bands
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get instrument tick bands

Get instrument tick bands information

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/instrument-tick-bands`

> Request Example

```shell
GET /api/v5/public/instrument-tick-bands?instType=OPTION
```

```shell
GET /api/v5/public/instrument-tick-bands?instType=EVENTS
```

#### Request Parameters

| Parameter      | Type   | Required    | Description                                     |
|:---------------| :----- |:------------|:------------------------------------------------|
| instType       | String | Yes | Instrument type<br>`OPTION`<br>`EVENTS`        |
| instFamily         | String | No   | Instrument family<br>Only applicable to `OPTION` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "OPTION",
            "instFamily": "BTC-USD",
            "tickBand": [
                {
                    "minPx": "0",
                    "maxPx": "100",
                    "tickSz": "0.1"
                },
                {
                    "minPx": "100",
                    "maxPx": "10000",
                    "tickSz": "1"
                }
            ]
        },
        {
            "instType": "OPTION",
            "instFamily": "ETH-USD",
            "tickBand": [
                {
                    "minPx": "0",
                    "maxPx": "100",
                    "tickSz": "0.1"
                },
                {
                    "minPx": "100",
                    "maxPx": "10000",
                    "tickSz": "1"
                }
            ]
        }
    ]
}

```

> Response Example: EVENTS

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "EVENTS",
            "instFamily": "",
            "tickBand": [
                {
                    "minPx": "0.001",
                    "maxPx": "0.04",
                    "tickSz": "0.001"
                },
                {
                    "minPx": "0.04",
                    "maxPx": "0.96",
                    "tickSz": "0.01"
                },
                {
                    "minPx": "0.96",
                    "maxPx": "0.999",
                    "tickSz": "0.001"
                }
            ]
        }
    ]
}

```

#### Response Parameters

| **Parameter** | **Type** | **Description**                      |
|:--------------|:---------|:-------------------------------------|
| instType      | String   | Instrument type                      |
| instFamily    | String   | Instrument family. Only applicable to `OPTION` |
| tickBand      | Array of objects    | Tick size band. For `EVENTS`, returns unified tick bands applicable to all event contracts. |
| > minPx       | String   | Minimum price while placing an order |
| > maxPx       | String   | Maximum price while placing an order |
| > tickSz      | String   | Tick size, e.g. `0.0001`             |
