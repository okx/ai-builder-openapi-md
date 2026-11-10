---
site: okex
lang: en
name: Candlesticks channel
type: guide
---

### Candlesticks channel

Retrieve the candlesticks data of an instrument. The push frequency is the fastest interval 1 second push the data.

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
   "op":"subscribe",
   "args":[
      {
         "channel":"sprd-candle1D",
         "sprdId":"BTC-USDT_BTC-USDT-SWAP"
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
         "channel":"sprd-candle1D",
         "sprdId":"BTC-USDT_BTC-USDT-SWAP"
      }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request parameters

| Parameter | Type | Required | Description |
|-----------| --- | --- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes | Operation, subscribe unsubscribe |
| args      | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name <br>`sprd-candle3M` `sprd-candle1M` <br>`sprd-candle1W` <br>`sprd-candle1D` `sprd-candle2D` `sprd-candle3D` `sprd-candle5D` <br>`sprd-candle12H` `sprd-candle6H` `sprd-candle4H` `sprd-candle2H` `sprd-candle1H` <br>`sprd-candle30m` `sprd-candle15m` `sprd-candle5m` `sprd-candle3m` `sprd-candle1m` <br>`sprd-candle3Mutc` `sprd-candle1Mutc` `sprd-candle1Wutc` `sprd-candle1Dutc` `sprd-candle2Dutc` `sprd-candle3Dutc` `sprd-candle5Dutc` `sprd-candle12Hutc` `sprd-candle6Hutc` |
| > sprdId  | String | Yes | Spread ID |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-candle1D",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-candle1D\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}

```

#### Response parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id           | String   | No       | Unique identifier of the message                        |
| event | String | Yes | Event, subscribe unsubscribe error |
| arg | Object | No | Subscribed channel |
| channel | String | yes | channel name |
| sprdId | String | Yes | Spread ID |
| code | String | No | Error code |
| msg | String | No | Error message |

> Push Data Example

```json
{
  "arg": {
    "channel": "sprd-candle1D",
    "sprdId": "BTC-USDT_BTC-USD-SWAP"
  },
  "data": [
    [
      "1597026383085",
      "8533.02",
      "8553.74",
      "8527.17",
      "8548.26",
      "45247",
      "0"
    ]
  ]
}

```

#### Push data parameters

| Parameter | Type | Description |
| --- | --- | --- |
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > sprdId | String | Spread ID |
| data | Array of Arrays | Subscribed data |
| > ts | String | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| > o | String | Open price |
| > h | String | highest price |
| > l | String | Lowest price |
| > c | String | Close price |
| > vol | String | Trading volume, in szCcy |
| > confirm | String | The state of candlesticks.0 represents that it is uncompleted, 1 represents that it is completed. |

<aside class="notice">
The data returned will be arranged in an array like this: [ts,o,h,l,c,vol,confirm]
</aside>
