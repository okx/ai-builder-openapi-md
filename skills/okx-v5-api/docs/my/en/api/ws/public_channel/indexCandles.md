---
site: my
lang: en
name: Index candlesticks channel
type: guide
---

### Index candlesticks channel

Retrieve the candlesticks data of the index. The push frequency is the fastest interval 1 second push the data. .

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "index-candle30m",
      "instId": "BTC-USD"
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
          "channel": "index-candle30m",
          "instId": "BTC-USD"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| :-------- | :----- | :------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                                                                                                                                                                                                                                                                                                                                                                                              |
| args    | Array of objects  | Yes      | List of subscribed channels                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > channel | String | Yes      | Channel name <br>`index-candle3M` <br>`index-candle1M` <br>`index-candle1W` <br>`index-candle1D` <br>`index-candle2D` <br>`index-candle3D` <br>`index-candle5D` <br>`index-candle12H` <br>`index-candle6H` <br>`index-candle4H` <br>`index -candle2H` <br>`index-candle1H` <br>`index-candle30m` <br>`index-candle15m` <br>`index-candle5m` <br>`index-candle3m` <br>`index-candle1m` <br>`index-candle3Mutc`  <br>`index-candle1Mutc` <br>`index-candle1Wutc` <br>`index-candle1Dutc` <br>`index-candle2Dutc` <br>`index-candle3Dutc` <br>`index-candle5Dutc` <br>`index-candle12Hutc` <br>`index-candle6Hutc`|
| > instId  | String | Yes      | Index, e.g. `BTC-USD`<br>Same as `uly`.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "index-candle30m",
    "instId": "BTC-USD"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"index-candle30m\", \"instId\" : \"BTC-USD\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description               |
| :-------- | :----- | :------- | ------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | `subscribe` `unsubscribe` |
| arg     | Object | No       | Subscribed channel        |
| > channel | String | Yes      | Channel name              |
| > instId  | String | No       | Index, e.g. `BTC-USD`             |
| code      | String | No       | Error code                |
| msg       | String | No       | Error message             |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "index-candle30m",
    "instId": "BTC-USD"
  },
  "data": [["1597026383085", "3811.31", "3811.31", "3811.31", "3811.31", "0"]]
}
```

#### Push data parameters

| Parameter | Type   | Description                                                                       |
| :-------- | :----- | --------------------------------------------------------------------------------- |
| arg     | Object | Successfully subscribed channel                                                   |
| > channel | String | Channel name                                                                      |
| > instId  | String | Index                                                                     |
| data    | Array of Arrays  | Subscribed data                                                                   |
| > ts      | String | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > o       | String | Open price                                                                        |
| > h       | String | Highest price                                                                     |
| > l       | String | Lowest price                                                                      |
| > c       | String | Close price                                                                       |
| > confirm     | String  | The state of candlesticks.<br>`0` represents that it is uncompleted, `1` represents that it is completed. |

<aside class="notice">
The order of the returned values is: [ts,o,h,l,c,confirm]
</aside>
