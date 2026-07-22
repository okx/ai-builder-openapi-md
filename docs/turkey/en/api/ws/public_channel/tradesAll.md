---
site: turkey
lang: en
name: WS / All trades channel
type: guide
---

### WS / All trades channel

Retrieve the recent trades data. Data will be pushed whenever there is a trade. Every update contain only one trade. 

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "trades-all",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "trades-all",
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

| Parameter | Type   | Required | Description                          |
| :-------- | :----- | :------- | ------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args    | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`trades-all`               |
| > instId  | String | Yes      | Instrument ID                        |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "trades-all",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"trades-all\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
| :-------- | :----- |:---------| ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                       |
| > channel | String | Yes      | Channel name                             |
| > instId  | String | Yes      | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes      | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "trades-all",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "tradeId": "130639474",
      "px": "42219.9",
      "sz": "0.12060306",
      "side": "buy",
      "source": "0",
      "ts": "1630048897897"
    }
  ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                          |
|:--------------| :------- |--------------------------------------------------------------------------|
| arg           | Object   | Successfully subscribed channel                                          |
| > channel     | String   | Channel name                                                             |
| > instId      | String   | Instrument ID                                                            |
| data          | Array of objects    | Subscribed data                                                          |
| > instId      | String   | Instrument ID, e.g. `BTC-USDT`                                           |
| > tradeId     | String   | Trade ID                                                                 |
| > px          | String   | Trade price                                                              |
| > sz          | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| > side        | String   | Trade direction<br>`buy`<br>`sell`                                       |
| > source | String | Order source<br> `0`: normal<br>`1`: Enhanced Liquidity Program order |
| > ts          | String   | Filled time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
