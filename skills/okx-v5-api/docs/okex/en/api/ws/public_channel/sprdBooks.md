---
site: okex
lang: en
name: WebSocket Public Channel
type: guide
---

## WebSocket Public Channel

- Production Trading URL: `wss://ws.okx.com:8443/ws/v5/business`
- Demo Trading URL: `wss://wspap.okx.com:8443/ws/v5/business` 

### Order book channel

Retrieve order book data. Available channels:

* `sprd-bbo-tbt`: 1 depth level snapshot will be pushed in the initial push. Snapshot data will be pushed every 10 ms when there are changes in the 1 depth level snapshot.
* `sprd-books5`: 5 depth levels snapshot will be pushed in the initial push. Snapshot data will be pushed every 100 ms when there are changes in the 5 depth levels snapshot.
* `sprd-books-l2-tbt`: 400 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 10 ms for the changes in the order book during that period of time. 
* The push sequence for order book channels within the same connection and trading symbols is fixed as: sprd-bbo-tbt -> sprd-books-l2-tbt -> sprd-books5.

#### URL Path

/ws/v5/business

> Request Example: sprd-books5

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-books5",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "sprd-books5",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

> Request Example: sprd-books-l2-tbt

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-books-l2-tbt",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "sprd-books-l2-tbt",
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

| Parameter | Type | Required | Description                                                        |
|:----------| :--- | :--- |--------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes | Operation<br>`subscribe`<br>`unsubscribe`<br>                      |
| args      | Array of objects | Yes | List of subscribed channels<br>`sprd-bbo-tbt`<br>`sprd-books5`<br>`sprd-books-l2-tbt` |
| > channel | String | Yes | Channel name                                                       |
| > sprdId  | String | Yes | spread ID                                                          |

> Successful Response Example: sprd-books5

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-books5",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
  },
  "connId": "a4d3ae55"
}
```

> Successful Response Example: sprd-books-l2-tbt

```json
{
  "id": "1512",
   "event":"subscribe",
   "arg":{
      "channel":"sprd-books-l2-tbt",
      "sprdId":"BTC-USDT_BTC-USDT-SWAP"
   },
   "connId":"214fdd24"
}
```

> Failure Response Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"args\":[{ \"channel\" : \"sprd-books5\", \"sprdId\" : \"BTC-USD_BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type     | Required | Description                                                                   |
|:----------|:---------|:---------|-------------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String   | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                              |
| arg       | Object   | No       | Subscribed channels<br>`sprd-bbo-tbt`<br>`sprd-books5`<br>`sprd-books-l2-tbt` |
| > channel | String   | Yes      | Channel name                                                                  |
| > sprdId  | String   | Yes      | spread ID                                                                     |
| msg       | String   | No       | Error message                                                                 |
| code      | String   | No       | Error code                                                                    |
| connId    | String   | Yes      | WebSocket connection ID                                                       |

> Push Data Example: sprd-books5

```json
{
  "arg": {
    "channel": "sprd-books5",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        ["111.06","55154","2"],
        ["111.07","53276","2"],
        ["111.08","72435","2"],
        ["111.09","70312","2"],
        ["111.1","67272","2"]],
      "bids": [
        ["111.05","57745","2"],
        ["111.04","57109","2"],
        ["111.03","69563","2"],
        ["111.02","71248","2"],
        ["111.01","65090","2"]],
      "ts": "1670324386802",
      "seqId":1724294007352168320
    }
  ]
}
```

> Push Data Example: sprd-books-l2-tbt

```json
{
   "arg":{
      "channel":"sprd-books-l2-tbt",
      "sprdId":"BTC-USDT_BTC-USDT-SWAP"
   },
   "action":"snapshot",
   "data":[
      {
         "asks":[
            ["1.9","1.1","3"],
            ["2.5","0.9","1"],
            ["3.2","4.921","1"],
            ["4.8","0.165","1"],
            ["5.2","4.921","1"]
          ......
         ],
         "bids":[
            ["1.8","0.165","1"],
            ["0.6","0.2","2"],
            ["0","23.49","1"],
            ["-0.1","1","1"],
            ["-0.6","1","1"],
            ["-3.9","4.921","1"]
            ......
         ],
         "ts":"1724391380926",
         "checksum":-1285595583,
         "prevSeqId":-1,
         "seqId":1724294007352168320
      }
   ]
}
```

#### Push data parameters

| Parameter | Type | Description |
| --- | --- | --- |
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > sprdId | String | spread ID |
| action | String | Push data action, incremental data or full snapshot.<br>`snapshot`: full<br>`update`: incremental |
| data | Array of objects | Subscribed data |
| > asks | Array of strings | Order book on sell side |
| > bids | Array of strings | Order book on buy side |
| > ts | String | Order book generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| > checksum | Integer | Checksum, implementation details below. Only applicable to `sprd-books-l2-tbt`. |
| > prevSeqId | Integer | Sequence ID of the last sent message. Only applicable to `sprd-books-l2-tbt`. |
| > seqId | Integer | Sequence ID of the current message, implementation details below. |

<aside class="notice">
An example of the array of asks and bids values: ["411.8", "10", "4"]<br>
- "411.8" is the depth price<br>
- "10" is the quantity at the price (Unit: szCcy)<br>
- "4" is the number of orders at the price.<br>
</aside>

#### Sequence ID

`seqId` is the sequence ID of the market data published. The set of sequence ID received by users is the same if users are connecting to the same channel through multiple websocket connections. Each `sprdId` has an unique set of sequence ID. Users can use `prevSeqId` and `seqId` to build the message sequencing for incremental order book updates. Generally the value of seqId is larger than prevSeqId. The `prevSeqId` in the new message matches with `seqId` of the previous message. The smallest possible sequence ID value is 0, except in snapshot messages where the prevSeqId is always -1.<br>

Exceptions:<br>
1. If there are no updates to the depth for an extended period, OKX will send a message with `'asks': [], 'bids': []` to inform users that the connection is still active. `seqId` is the same as the last sent message and `prevSeqId` equals to `seqId`.
2. The sequence number may be reset due to maintenance, and in this case, users will receive an incremental message with `seqId` smaller than `prevSeqId`. However, subsequent messages will follow the regular sequencing rule.

##### Example
1. Snapshot message: prevSeqId = -1, seqId = 10
2. Incremental message 1 (normal update): prevSeqId = 10, seqId = 15
3. Incremental message 2 (no update): prevSeqId = 15, seqId = 15
4. Incremental message 3 (sequence reset): prevSeqId = 15, seqId = 3
5. Incremental message 4 (normal update): prevSeqId = 3, seqId = 5

#### Checksum

This mechanism can assist users in checking the accuracy of depth data.

##### Merging incremental data into full data

After subscribing to the incremental load push (such as `books` 400 levels) of Order Book Channel, users first receive the initial full load of market depth. After the incremental load is subsequently received, update the local full load.

1. If there is the same price, compare the size. If the size is 0, delete this depth data. If the size changes, replace the original data.
2. If there is no same price, sort by price (bid in descending order, ask in ascending order), and insert the depth information into the full load.

##### Calculate Checksum
Use the first 25 bids and asks in the full load to form a string (where a colon connects the price and size in an ask or a bid), and then calculate the CRC32 value (32-bit signed integer).
