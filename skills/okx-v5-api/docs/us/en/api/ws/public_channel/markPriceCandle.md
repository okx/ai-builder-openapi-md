---
site: us
lang: en
name: Mark price candlesticks channel
type: guide
---

### Mark price candlesticks channel

Retrieve the candlesticks data of the mark price. The push frequency is the fastest interval 1 second push the data.

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "mark-price-candle1D",
      "instId": "BTC-USD-190628"
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
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "mark-price-candle1D",
          "instId": "BTC-USD-190628"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| :-------- | :----- | :------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe` `unsubscribe`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| args    | Array of objects  | Yes      | List of subscribed channels                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > channel | String | Yes      | Channel name  <br>`mark-price-candle3M`  <br>`mark-price-candle1M`  <br>`mark-price-candle1W`  <br>`mark-price-candle1D` <br>`mark-price-candle2D` <br>`mark-price-candle3D`  <br>`mark-price-candle5D` <br>`mark-price-candle12H`  <br>`mark-price-candle6H`  <br>`mark-price-candle4H` <br>`mark-price-candle2H` <br>`mark-price-candle1H`  <br>`mark-price-candle30m` <br>`mark-price-candle15m`  <br>`mark-price-candle5m`  <br>`mark-price-candle3m`  <br>`mark-price-candle1m`  <br>`mark-price-candle1Yutc` <br>`mark-price-candle3Mutc` <br>`mark-price-candle1Mutc`  <br>`mark-price-candle1Wutc` <br>`mark-price-candle1Dutc` <br>`mark-price-candle2Dutc` <br>`mark-price-candle3Dutc` <br>`mark-price-candle5Dutc`  <br>`mark-price-candle12Hutc` <br>`mark-price-candle6Hutc`  |
| > instId  | String | Yes      | Instrument ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "mark-price-candle1D",
    "instId": "BTC-USD-190628"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"mark-price-candle1D\", \"instId\" : \"BTC-USD-190628\"}]}",
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
| > instId  | String | Yes      | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "mark-price-candle1D",
    "instId": "BTC-USD-190628"
  },
  "data": [
    ["1597026383085", "3.721", "3.743", "3.677", "3.708","0"],
    ["1597026383085", "3.731", "3.799", "3.494", "3.72","1"]
  ]
}
```

#### Push data parameters

| Parameter | Type   | Description                                                                       |
| :-------- | :----- | --------------------------------------------------------------------------------- |
| arg     | Object | Successfully subscribed channel                                                   |
| > channel | String | Channel name                                                                      |
| > instId  | String | Instrument ID                                                                     |
| data    | Array of Arrays  | Subscribed data                                                                   |
| > ts      | String | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > o       | String | Open price                                                                        |
| > h       | String | Highest price                                                                     |
| > l       | String | Lowest price                                                                      |
| > c       | String | Close price                                                                       |
| > confirm     | String  | The state of candlesticks.<br>`0` represents that it is uncompleted, `1` represents that it is completed. |
