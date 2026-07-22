---
site: my
lang: en
name: Open interest channel
type: guide
---

### Open interest channel

Retrieve the open interest. Data will be pushed every 3 seconds when there are updates.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "open-interest",
      "instId": "LTC-USD-SWAP"
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
    args = [
        {
          "channel": "open-interest",
          "instId": "LTC-USD-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                          |
| :-------- | :----- | :------- | ------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args    | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`open-interest`        |
| > instId  | String | Yes      | Instrument ID                        |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "open-interest",
      "instId": "LTC-USD-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"open-interest\", \"instId\" : \"LTC-USD-SWAP\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                               |
| :-------- | :----- | :------- | ----------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                        |
| > channel | String | Yes      | Channel name                              |
| > instId  | String | Yes      | Instrument ID                             |
| code      | String | No       | Error code                                |
| msg       | String | No       | Error message                             |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg": {
        "channel": "open-interest",
        "instId": "BTC-USDT-SWAP"
    },
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "oi": "2216113.01000000309",
            "oiCcy": "22161.1301000000309",
            "oiUsd": "1939251795.54769270396321",
            "ts": "1743041250440"
        }
    ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                                 |
| ------------- | -------- |-------------------------------------------------------------------------------------------------|
| arg         | Object   | Successfully subscribed channel                                                                 |
| > channel       | String   | Channel name                                                                                    |
| > instId        | String   | Instrument ID                                                                                   |
| data        | Array of objects    | Subscribed data                                                                                 |
| > instType      | String   | Instrument type                                                                                 |
| > instId        | String   | Instrument ID, e.g. `BTC-USDT-SWAP`                                                             |
| > oi            | String   | Open interest, in units of contracts.                                                           |
| > oiCcy         | String   | Open interest, in currency units, like BTC.                                                     |
| > oiUsd   | String | Open interest in number of USD |
| > ts            | String   | The time when the data was updated, Unix timestamp format in milliseconds, e.g. `1597026383085` |
