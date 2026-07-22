---
site: hongkong
lang: en
name: WS / Status channel
type: guide
---

## WS / Status channel

Get the status of system maintenance and push when rescheduling and the system maintenance status and end time changes. First subscription: "Push the latest change data"; every time there is a state change, push the changed content.

Planned system maintenance that may result in short interruption (lasting less than 5 seconds) or websocket disconnection (users can immediately reconnect) will not be announced. The maintenance will only be performed during times of low market volatility.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "status"
    }
  ]
}
```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/public")
    await ws.start()
    args = [{
        "channel": "status"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                      |
| :-------- | :----- | :------- | ---------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | `subscribe` `unsubscribe`                                        |
| args    | Array of objects  | Yes      | List of subscribed channels                                      |
| > channel | String | Yes      | Channel name<br>`status`                                    |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "status"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"statuss\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                       |
| :-------- | :----- | :------- | --------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | `subscribe` `unsubscribe` `error` |
| arg     | Object | No       | Subscribed channel                |
| > channel | String | Yes      | Channel name<br>`status`            |
| code      | String | No       | Error code                        |
| msg       | String | No       | Error message                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg": {
        "channel": "status"
    },
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672825980000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "0",
            "state": "completed",
            "system": "unified",
            "maintType": "1",
            "env": "1",
            "title": "Trading account WebSocket system upgrade",
            "ts": "1672826038470"
        }
    ]
}
```

#### Push data parameters

| Parameter      | Type   | Description                                                                                                                                                                                                                                                                      |
|:---------------| :----- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg            | Object | Successfully subscribed channel                                                                                                                                                                                                                                                  |
| > channel      | String | Channel name                                                                                                                                                                                                                                                                     |
| data           | Array of objects  | Subscribed data                                                                                                                                                                                                                                                                  |
| > title        | String | The title of system maintenance instructions                                                                                                                                                                                                                                     |
| > state        | String | System maintenance status,`scheduled`: waiting;  `ongoing`: processing; `pre_open`: pre_open; `completed`: completed ;`canceled`: canceled. <br>Generally, `pre_open` last about 10 minutes. There will be `pre_open` when the time of upgrade is too long.                     |
| > begin        | String | Start time of system maintenance, Unix timestamp format in milliseconds, e.g. `1617788463867`                                                                                                                                                                                    |
| > end          | String | Time of resuming trading totally. Unix timestamp format in milliseconds, e.g. `1617788463867`.<br>It is expected end time before `completed`, changed to actual end time after `completed`.                                                                                     |
| > preOpenBegin | String | The time of pre_open. Canceling orders, placing Post Only orders, and transferring funds to trading accounts are back after `preOpenBegin`.                                                                                                                                      |
| > href         | String | Hyperlink for system maintenance details, if there is no return value, the default value will be empty. e.g. “”                                                                                                                                                                  |
| > serviceType  | String | Service type, `0`: WebSocket ; `5`: Trading service; `6`: Block trading; `7`: Trading bot; `8`: Trading service (in batches of accounts); `9`: Trading service (in batches of products); `10`: Spread trading; `11`: Copy trading; `99`: Others (e.g. Suspend partial instruments) |
| > system       | String | System, `unified`: Trading account                                                                                                                                                                                                                                                |
| > scheDesc     | String | Rescheduled description, e.g. `Rescheduled from 2021-01-26T16:30:00.000Z to 2021-01-28T16:30:00.000Z`                                                                                                                                                                            |
| > maintType    | String | Maintenance type<br>`1`: Scheduled maintenance; `2`: Unscheduled maintenance; `3`: System disruption                                                                                                                                                                            |
| > env          | String | Environment.<br>`1`: Production Trading, `2`: Demo Trading                                                                                                                                                                                                                      |
| > ts      | String | Push time due to change event, Unix timestamp format in milliseconds, e.g. `1617788463867`                                                                                                                                                                                       |
