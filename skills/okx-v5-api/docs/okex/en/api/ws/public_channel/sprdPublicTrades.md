---
site: okex
lang: en
name: Public Trades channel
type: guide
---

### Public Trades channel

Retrieve the recent trades data from `sprd-public-trades`. Data will be pushed whenever there is a trade. Every update contains only one trade.

#### URL Path

/ws/v5/business

> Request Example 

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-public-trades",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
          "channel": "sprd-public-trades",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type | Required | Description |
|:----------| :--- | :--- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>`sprd-public-trades` |
| > sprdId  | String | Yes | spread ID |

> Successful Response Example

```shell
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "sprd-public-trades",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    },
  "connId": "a4d3ae55"
}
```

> Failure Response Example

```shell
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-public-trades\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | --- |
| id           | String   | No       | Unique identifier of the message                        |
| event | String | Yes | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name |
| > sprdId | String | Yes | spread ID |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```shell
{
    "arg": {
        "channel": "sprd-public-trades",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    },
    "data": [
        {
            "sprdId": "BTC-USDT_BTC-USDT-SWAP",
            "tradeId": "2499206329160695808",
            "px": "-10",
            "sz": "0.001",
            "side": "sell",
            "ts": "1726801105519"
        }
    ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > sprdId | String | spread ID |
| data | Array of objects | Subscribed data |
| > sprdId | String | spread ID, e.g. |
| > tradeId | String | Trade ID |
| > px | String | Trade price |
| sz         | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| > side | String | Trade direction, buy, sell |
| > ts | String | Filled time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
