---
site: okex
lang: en
name: WS / Option trades channel
type: guide
---

### WS / Option trades channel

Retrieve the recent trades data. Data will be pushed whenever there is a trade. Every update contain only one trade.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    }]
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
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```
#### Request Parameters

| Parameter    | Type   | Required    | Description                                                                                                                       |
|:-------------| :----- |:------------|-----------------------------------------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op           | String | Yes         | `subscribe` `unsubscribe`                                                                                                         |
| args         | Array of objects  | Yes         | List of subscribed channels                                                                                                       |
| > channel    | String | Yes         | Channel name<br>`option-trades`                                                                                                     |
| > instType   | String | Yes         | Instrument type, `OPTION`                                                                                                       |
| > instId     | String | Conditional | Instrument ID, e.g. BTC-USD-221230-4000-C, Either `instId` or `instFamily` is required. If both are passed, `instId` will be used. |
| > instFamily | String | Conditional          | Instrument family, e.g. BTC-USD                                                                                                   |

> Successful Response Example

```json
{
  "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"option-trades\"}]}",
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
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    },
    "data": [
        {
            "fillVol": "0.5066007836914062",
            "fwdPx": "16469.69928595038",
            "idxPx": "16537.2",
            "instFamily": "BTC-USD",
            "instId": "BTC-USD-230224-18000-C",
            "markPx": "0.04690107010619562",
            "optType": "C",
            "px": "0.045",
            "side": "sell",
            "sz": "2",
            "tradeId": "38",
            "ts": "1672286551080"
        }
    ]
}
```

#### Push data parameters

| Parameter    | Type   | Description                                                                                                                                                                                                                                                  |
|:-------------| :----- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg          | Object | Successfully subscribed channel                                                                                                                                                                                                                              |
| > channel    | String | Channel name                                                                                                                                                                                                                                                 |
| data         | Array of objects  | Subscribed data                                                                                                                                                                                                                                              |
| > instId     | String   | Instrument ID                                                            |
| > instFamily | String   | Instrument family                                                        |
| > tradeId    | String   | Trade ID                                                                 |
| > px         | String   | Trade price                                                              |
| > sz         | String   | Trade quantity. The unit is contract.                                  |
| > side       | String   | Trade side <br>`buy`  <br>`sell`                                        |
| > optType    | String   | Option type, C: Call P: Put                                              |
| > fillVol    | String   | Implied volatility while trading (Correspond to trade price)             |
| > fwdPx      | String   | Forward price while trading                                              |
| > idxPx      | String   | Index price while trading                                                |
| > markPx     | String   | Mark price while trading                                         |
| > ts         | String   | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |

<aside class="notice">
The first data you receive after subscribing may be cached from the previous trade, so please ignore it.
</aside>
