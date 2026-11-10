---
site: okex
lang: en
name: Tickers channel
type: guide
---

### Tickers channel

Retrieve the last traded price, bid price, ask price.
The fastest rate is 1 update/100ms. There will be no update if the event is not triggered. The events which can trigger update: trade, the change on best ask/bid price

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-tickers",
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
          "channel": "sprd-tickers",
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
| :--- | :--- | :--- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>`sprd-tickers` |
| > sprdId | String | Yes | spread ID |

> Successful Response Example

```shell
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-tickers",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-tickers\", \"instId\" : \"LTC-USD-200327\"}]}",
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
        "channel": "sprd-tickers",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    },
    "data": [
        {
            "sprdId": "BTC-USDT_BTC-USDT-SWAP",
            "last": "4",
            "lastSz": "0.01",
            "askPx": "19.7",
            "askSz": "5.79",
            "bidPx": "5.9",
            "bidSz": "5.79",
            "open24h": "-7",
            "high24h": "19.6",
            "low24h": "-7",
            "vol24h": "9.87",
            "ts": "1715247061026"
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
| > sprdId | String | spread ID |
| > last | String | Last traded price |
| > lastSz | String | Last traded size |
| > askPx | String | Best ask price |
| > askSz | String | Best ask size |
| > bidPx | String | Best bid price |
| > bidSz | String | Best bid size |
| > open24h        | String      | Open price in the past 24 hours                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > high24h        | String      | Highest price in the past 24 hours                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > low24h         | String      | Lowest price in the past 24 hours                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > vol24h         | String      | 24h trading volume, with a unit of base currency or USD                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > ts | String | Ticker data generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |

<aside class="notice">
vol24h<br>
For Spot vs USDT-margined contracts spread and USDT-margined contracts spread, the volume is with the unit of base currency; for Crypto-margined contracts spread, the volume is with the unit of USD.
</aside>
