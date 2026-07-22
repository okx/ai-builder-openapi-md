---
site: my
lang: en
name: Index tickers channel
type: guide
---

### Index tickers channel

Retrieve index tickers data. Push data every 100ms if there are any changes, otherwise push once a minute.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "index-tickers",
      "instId": "BTC-USDT"
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
          "channel": "index-tickers",
          "instId": "BTC-USDT"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                            |
| :-------- | :----- | :------- |------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | `subscribe` `unsubscribe`                                              |
| args    | Array of objects  | Yes      | List of subscribed channels                                            |
| > channel | String | Yes      | Channel name<br>`index-tickers`                                          |
| > instId  | String | Yes      | Index with USD, USDT, BTC, USDC as the quote currency, e.g. `BTC-USDT`<br>Same as `uly`. |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "index-tickers",
    "instId": "BTC-USDT"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"index-tickers\", \"instId\" : \"BTC-USDT\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                                            |
| :-------- | :----- | :------- |------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | `subscribe` `unsubscribe` `error`                                      |
| arg     | Object | No       | Subscribed channel                                                     |
| > channel | String | Yes      | Channel name<br>`index-tickers`                                          |
| > instId  | String | Yes       | Index with USD, USDT, BTC, USDC as the quote currency, e.g. `BTC-USDT` |
| code      | String | No       | Error code                                                             |
| msg       | String | No       | Error message                                                          |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "index-tickers",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "idxPx": "0.1",
      "high24h": "0.5",
      "low24h": "0.1",
      "open24h": "0.1",
      "sodUtc0": "0.1",
      "sodUtc8": "0.1",
      "ts": "1597026383085"
    }
  ]
}
```

#### Push data parameters

| Parameter | Type   | Description                                                              |
| :-------- | :----- | ------------------------------------------------------------------------ |
| arg     | Object | Successfully subscribed channel                                          |
| > channel | String | Channel name                                                             |
| > instId  | String | Index with USD, USDT, or BTC as quote currency, e.g. `BTC-USDT`.         |
| data    | Array of objects  | Subscribed data                                                          |
| > instId  | String | Index                                                                    |
| > idxPx   | String | Latest Index Price                                                       |
| > open24h | String | Open price in the past 24 hours                                          |
| > high24h | String | Highest price in the past 24 hours                                       |
| > low24h  | String | Lowest price in the past 24 hours                                        |
| > sodUtc0 | String | Open price in the UTC 0                                                  |
| > sodUtc8 | String | Open price in the UTC 8                                                  |
| > ts      | String | Update time of the index ticker, Unix timestamp format in milliseconds, e.g. `1597026383085` |
