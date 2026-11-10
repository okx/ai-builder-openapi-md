---
site: us
lang: en
name: Mark price channel
type: guide
---

### Mark price channel

Retrieve the mark price. Data will be pushed every 200 ms when the mark price changes, and will be pushed every 10 seconds when the mark price does not change.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "mark-price",
      "instId": "LTC-USD-190628"
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
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [{
		"channel": "mark-price",
		"instId": "BTC-USDT"
	}]

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
| > channel | String | Yes      | Channel name<br>`mark-price`           |
| > instId  | String | Yes      | Instrument ID                        |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "mark-price",
    "instId": "LTC-USD-190628"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"mark-price\", \"instId\" : \"LTC-USD-190628\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
| :-------- | :----- | :------- | ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                       |
| > channel | String | Yes      | Channel name                             |
| > instId  | String | No       | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "mark-price",
    "instId": "LTC-USD-190628"
  },
  "data": [
    {
      "instType": "FUTURES",
      "instId": "LTC-USD-190628",
      "markPx": "0.1",
      "ts": "1597026383085"
    }
  ]
}
```

#### Push data parameters

| Parameter  | Type   | Description                                                                    |
| :--------- | :----- | ------------------------------------------------------------------------------ |
| arg      | Object | Successfully subscribed channel                                                |
| > channel  | String | Channel name                                                                   |
| > instId   | String | Instrument ID                                                                  |
| data     | Array of objects  | Subscribed data                                                                |
| > instType | String | Instrument type                                                                |
| > instId   | String | Instrument ID                                                                  |
| > markPx   | String | Mark price                                                                     |
| > ts       | String | Price update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |

<aside class="notice">
In rare cases, two mark price messages with the same timestamp may be received within a short window. This can occur during scheduled system maintenance or deployments and is not persistent. When this happens, clients should use the later-received message as the authoritative value. The difference between the two values will be negligible and will not materially affect trading strategies.
</aside>
