---
site: turkey
lang: en
name: WS / Trades channel
type: guide
---

### WS / Trades channel

Retrieve the recent trades data. Data will be pushed whenever there is a trade. Every update may aggregate multiple trades. <br><br>

The message is sent only once per taker order, filled price, source. The count field is used to represent the number of aggregated matches.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "trades",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "trades",
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

| Parameter   | Type                | Required | Description                          |
|:------------|:--------------------| :------- | ------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op          | String              | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args        | Array of objects    | Yes      | List of subscribed channels          |
| > channel   | String              | Yes      | Channel name<br>`trades`               |
| > instId    | String              | Yes      | Instrument ID                        |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "trades",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"trades\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
|:----------| :----- | :------- | ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                       |
| > channel | String | Yes      | Channel name                             |
| > instId  | String | Yes      | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId    | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "trades",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "tradeId": "130639474",
      "px": "42219.9",
      "sz": "0.12060306",
      "side": "buy",
      "ts": "1630048897897",
      "count": "3",
      "source": "0",
      "seqId": 1234
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
| > tradeId     | String   | The last trade ID in the trades aggregation                              |
| > px          | String   | Trade price                                                              |
| > sz          | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| > side        | String   | Trade side of taker<br>`buy`<br>`sell`                                       |
| > ts          | String   | Filled time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > count       | String   | The count of trades aggregated                                           |
| > source | String	 | Order source<br> `0`: normal orders<br>`1`: Enhanced Liquidity Program order |
| > seqId     | Integer |  Sequence ID of the current message.  |

<aside class="notice">
Aggregation function description:<br>
1. The system will send only one message per taker order, filled price, source. The `count` field will be used to represent the number of aggregated matches.<br>
2. The `tradeId` field in the message becomes the last trade ID in the aggregation.<br>
3. When the `count` = 1, it means the taker order matches only one maker order with the specific price.<br>
4. When the `count` > 1, it means the taker order matches multiple maker orders with the same price. For example, if `tradeId` = 123 and `count` = 3, it means the message aggregates the trades of `tradeId` = 123, 122, and 121. Maker side has filled multiple orders.<br>
5. Users can use this information to compare with data from the `trades-all` channel.<br>
6. Order book and the aggregated trades data are still published sequentially.<br>
</aside>

<aside class="notice">
The seqId may be the same for different trade updates that occur at the same time.
</aside>
