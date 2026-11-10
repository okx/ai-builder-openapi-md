---
site: okex
lang: en
name: GET / Status
method: GET
path: /api/v5/system/status
module: system
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

# Status

## GET / Status

Get event status of system upgrade.

Planned system maintenance that may result in short interruption (lasting less than 5 seconds) or websocket disconnection (users can immediately reconnect) will not be announced. The maintenance will only be performed during times of low market volatility.

#### Rate Limit: 1 request per 5 seconds

#### HTTP Request

`GET /api/v5/system/status`

> Request Example

```shell
GET /api/v5/system/status

GET /api/v5/system/status?state=canceled

```

```python
import okx.Status as Status

flag = "0"  # Production trading: 0, Demo trading: 1
statusAPI = Status.StatusAPI(
    domain="https://openapi.okx.com",
    flag=flag,
)

# Get event status of system upgrade
result = statusAPI.status()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------| :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| state     | String | No       | System maintenance status<br>`scheduled`: waiting<br>`ongoing`: processing<br>`pre_open`: pre_open<br>`completed`: completed<br>`canceled`: canceled<br>Generally, `pre_open` last about 10 minutes. There will be `pre_open` when the time of upgrade is too long.  <br>If this parameter is not filled, the data with status `scheduled`, `ongoing` and `pre_open` will be returned by default |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672823520000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "8",
            "state": "completed",
            "maintType": "1",
            "env": "1",
            "system": "unified",
            "title": "Trading account system upgrade (in batches of accounts)"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter    | Type   | Description                                                                                                                                                                                                                                                                                                                                                |
|:-------------| :----- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title        | String | The title of system maintenance instructions                                                                                                                                                                                                                                                                                                               |
| state        | String | System maintenance status                                                                                                                                                                                                                                                                                                                                  |
| begin        | String | Begin time of system maintenance, Unix timestamp format in milliseconds, e.g. `1617788463867`                                                                                                                                                                                                                                                              |
| end          | String | Time of resuming trading totally. Unix timestamp format in milliseconds, e.g. `1617788463867`.<br>It is expected end time before `completed`, changed to actual end time after `completed`.                                                                                                                                                               |
| preOpenBegin | String | The time of pre_open. Canceling orders, placing Post Only orders, and transferring funds to trading accounts are back after `preOpenBegin`.                                                                                                                                                                                                                |
| href         | String | Hyperlink for system maintenance details, if there is no return value, the default value will be empty. e.g. ""                                                                                                                                                                                                                                              |
| serviceType  | String | Service type<br>`0`: WebSocket<br>`5`: Trading service<br>`6`: Block trading<br>`7`: Trading bot<br>`8`: Trading service (in batches of accounts)<br>`9`: Trading service (in batches of products)<br>`10`: Spread trading<br>`11`: Copy trading<br>`99`: Others (e.g. Suspend partial instruments)                                                          |
| system       | String | System<br>`unified`: Trading account                                                                                                                                                                                                                                                                                                                        |
| scheDesc     | String | Rescheduled description, e.g. `Rescheduled from 2021-01-26T16:30:00.000Z` to `2021-01-28T16:30:00.000Z`                                                                                                                                                                                                                                                    |
| maintType    | String | Maintenance type<br>`1`: Scheduled maintenance<br>`2`: Unscheduled maintenance<br>`3`: System disruption                                                                                                                                                                                                                                                   |
| env          | String | Environment<br>`1`: Production Trading<br>`2`: Demo Trading                                                                                                                                                                                                                                                                                                |
