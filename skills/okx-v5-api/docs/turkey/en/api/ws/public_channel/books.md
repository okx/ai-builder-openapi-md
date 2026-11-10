---
site: turkey
lang: en
name: WS / Order book channel
type: guide
---

### WS / Order book channel

Retrieve order book data. Best ask price may be lower than the best bid price during the pre-open period.<br>

Use `books` for 400 depth levels, `books5` for 5 depth levels, `bbo-tbt` tick-by-tick 1 depth level, `bbo-rpi-tbt` tick-by-tick 1 depth level of RPI orders only, `books50-l2-tbt` tick-by-tick 50 depth levels, and `books-l2-tbt` for tick-by-tick 400 depth levels. <br>

- `books`: 400 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 100 ms for the changes in the order book during that period of time. <br>
- `books-elp`: (Deprecated; use `books-rpi`) only push ELP orders. 400 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 100 ms for the changes in the order book during that period of time. <br>
- `books-rpi`: consolidated organic and RPI depth. 400 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 100 ms. No `checksum`; sequencing relies on `seqId`/`prevSeqId`. Each `asks`/`bids` element is `[price, totalQty, nonRpiQty, count]`. Supersedes `books-elp`.
- `books5`: 5 depth levels snapshot will be pushed in the initial push. Snapshot data will be pushed every 100 ms when there are changes in the 5 depth levels snapshot.<br>
- `bbo-tbt`: 1 depth level snapshot will be pushed in the initial push. Snapshot data will be pushed every 10 ms when there are changes in the 1 depth level snapshot. <br>
- `bbo-rpi-tbt`: only push RPI orders. 1 depth level snapshot will be pushed in the initial push. Snapshot data will be pushed every 10 ms when there are changes in the 1 depth level snapshot. <br>
- `books-l2-tbt`: 400 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 10 ms for the changes in the order book during that period of time. <br>
- `books50-l2-tbt`: 50 depth levels will be pushed in the initial full snapshot. Incremental data will be pushed every 10 ms for the changes in the order book during that period of time.
- The push sequence for order book channels within the same connection and trading symbols is fixed as: bbo-tbt -> bbo-rpi-tbt -> books-l2-tbt -> books50-l2-tbt -> books -> books-elp -> books-rpi -> books5.
- Users can not simultaneously subscribe to `books-l2-tbt` and `books50-l2-tbt/books` channels for the same trading symbol. 
  - For more details, please refer to the changelog [2024-07-17] (/docs-v5/log_en/#2024-07-17)

<aside class="notice">
Only API users who are VIP4 and above in trading fee tier are allowed to subscribe to "books-l2-tbt" 400 depth channels. Other users will receive error code 64003.<br>
Only API users who are VIP4 and above in trading fee tier are allowed to subscribe to "books50-l2-tbt" 50 depth channels. Other users will receive error code 64003.<br>
</aside>

Identity verification refers to [Login](/docs-v5/en/#overview-websocket-login) 

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "books",
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
        "channel": "books",
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

| Parameter | Type   | Required | Description                                                                            |
| :-------- | :----- | :------- |----------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                          |
| args    | Array of objects  | Yes      | List of subscribed channels                                                            |
| > channel | String | Yes      | Channel name<br>`books`<br>`books5`<br>`bbo-tbt`<br>`bbo-rpi-tbt`<br>`books50-l2-tbt`<br>`books-l2-tbt` |
| > instId  | String | Yes      | Instrument ID                                                                          |

> Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "connId": "a4d3ae55"
}
```

> Failure example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"books\", \"instId\" : \"BTC-USD-191227\"}]}",
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
| msg       | String | No       | Error message                            |
| code      | String | No       | Error code                               |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: Full Snapshot

```json
{
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "action": "snapshot",
  "data": [
    {
      "asks": [
        ["8476.98", "415", "0", "13"],
        ["8477", "7", "0", "2"],
        ["8477.34", "85", "0", "1"],
        ["8477.56", "1", "0", "1"],
        ["8505.84", "8", "0", "1"],
        ["8506.37", "85", "0", "1"],
        ["8506.49", "2", "0", "1"],
        ["8506.96", "100", "0", "2"]
      ],
      "bids": [
        ["8476.97", "256", "0", "12"],
        ["8475.55", "101", "0", "1"],
        ["8475.54", "100", "0", "1"],
        ["8475.3", "1", "0", "1"],
        ["8447.32", "6", "0", "1"],
        ["8447.02", "246", "0", "1"],
        ["8446.83", "24", "0", "1"],
        ["8446", "95", "0", "3"]
      ],
      "ts": "1597026383085",
      "checksum": 0,
      "prevSeqId": -1,
      "seqId": 123456
    }
  ]
}
```

> Push Data Example: Incremental Data

```json
{
  "arg": {
    "channel": "books",
    "instId": "BTC-USDT"
  },
  "action": "update",
  "data": [
    {
      "asks": [
        ["8476.98", "415", "0", "13"],
        ["8477", "7", "0", "2"],
        ["8477.34", "85", "0", "1"],
        ["8477.56", "1", "0", "1"],
        ["8505.84", "8", "0", "1"],
        ["8506.37", "85", "0", "1"],
        ["8506.49", "2", "0", "1"],
        ["8506.96", "100", "0", "2"]
      ],
      "bids": [
        ["8476.97", "256", "0", "12"],
        ["8475.55", "101", "0", "1"],
        ["8475.54", "100", "0", "1"],
        ["8475.3", "1", "0", "1"],
        ["8447.32", "6", "0", "1"],
        ["8447.02", "246", "0", "1"],
        ["8446.83", "24", "0", "1"],
        ["8446", "95", "0", "3"]
      ],
      "ts": "1597026383085",
      "checksum": 0,
      "prevSeqId": 123456,
      "seqId": 123457
    }
  ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                                         |
| :------------ | :------- | ------------------------------------------------------------------------------------------------------- |
| arg         | Object   | Successfully subscribed channel                                                                         |
| > channel     | String   | Channel name                                                                                            |
| > instId      | String   | Instrument ID                                                                                           |
| action      | String   | Push data action, incremental data or full snapshot. <br>`snapshot`: full  <br>` update`: incremental |
| data        | Array of objects    | Subscribed data                                                                                         |
| > asks        | Array of Arrays    | Order book on sell side<br>For `bbo-rpi-tbt`, only RPI orders are returned, including hidden RPI orders. Returns `[]` when there is no RPI order on the sell side.                                                                                 |
| > bids        | Array of Arrays    | Order book on buy side<br>For `bbo-rpi-tbt`, only RPI orders are returned, including hidden RPI orders. Returns `[]` when there is no RPI order on the buy side.                                                                                  |
| > ts          | String   | Order book generation time, Unix timestamp format in milliseconds, e.g. `1597026383085` <br>Exception: For the `bbo-tbt` and `bbo-rpi-tbt` channels, `ts` is the timestamp when the book is generated by matching engine. |
| > checksum    | Integer  | <del>Checksum</del> (Deprecated). The field remains in the push for `books`, `books-l2-tbt`, and `books50-l2-tbt`, but its value is fixed to `0` and must no longer be used for integrity verification. Please use `seqId/prevSeqId` to verify data continuity and accuracy. |
| > prevSeqId   | Integer  | Sequence ID of the last sent message. Only applicable to `books`, `books-l2-tbt`, `books50-l2-tbt` |
| > seqId       | Integer  | Sequence ID of the current message, implementation details below |

<aside class="notice">
An example of the array of asks and bids values: ["411.8", "10", "0", "4"]<br>
- "411.8" is the depth price<br>
- "10" is the quantity at the price (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)<br>
- "0" is part of a deprecated feature and it is always "0"<br>
- "4" is the number of orders at the price.
</aside>

<aside class="notice">
If you need to subscribe to many 50 or 400 depth level channels, it is recommended to subscribe through multiple websocket connections, with each of less than 30 channels.
</aside>

<aside class="notice">
The order book data will be updated around once a second during the call auction.
</aside>

<aside class="notice">
`books/books5/bbo-tbt/books-l2-tbt/books50-l2-tbt` don't return ELP orders<br>
`books-elp` only return ELP orders, including both valid and invalid parts (invalid parts means ELP buy orders with a price higher than best bid of non-ELP orders; or ELP sell orders with a price lower than best ask of non-ELP orders). Users should distinguish valid and invalid parts using the best bid/ask price of non-ELP orders.
</aside>

<aside class="notice">
`books-rpi` returns visible RPI orders only.<br>
`bbo-rpi-tbt` only returns RPI orders, including both visible and hidden parts. Hidden parts are RPI buy orders priced above the organic best ask, RPI sell orders priced below the organic best bid, or RPI buy and sell orders crossing each other within the organic spread. Use the organic best bid/ask from `bbo-tbt` to distinguish visible from hidden parts.<br>
An example of the array of asks and bids values for `bbo-rpi-tbt`: ["111.06", "55", "0", "2"]<br>
- "111.06" is the depth price<br>
- "55" is the quantity of RPI orders at the price (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)<br>
- "0" is part of a deprecated feature and it is always "0"<br>
- "2" is the number of RPI orders at the price.
</aside>

#### Sequence ID

`seqId` is the sequence ID of the market data published. The set of sequence ID received by users is the same if users are connecting to the same channel through multiple websocket connections. Each `instId` has an unique set of sequence ID. Users can use `prevSeqId` and `seqId` to build the message sequencing for incremental order book updates. Generally the value of seqId is larger than prevSeqId. The `prevSeqId` in the new message matches with `seqId` of the previous message. The smallest possible sequence ID value is 0, except in snapshot messages where the prevSeqId is always -1.<br>

Exceptions:<br>
1. If there are no updates to the depth for an extended period(Around 60 seconds), for the channel that always updates snapshot data, OKX will send the latest snapshot, for the channel that has incremental data, OKX will send a message with `'asks': [], 'bids': []` to inform users that the connection is still active. `seqId` is the same as the last sent message and `prevSeqId` equals to `seqId`.
2. The sequence number may be reset due to maintenance, and in this case, users will receive an incremental message with `seqId` smaller than `prevSeqId`. However, subsequent messages will follow the regular sequencing rule.

##### Example
1. Snapshot message: prevSeqId = -1, seqId = 10
2. Incremental message 1 (normal update): prevSeqId = 10, seqId = 15
3. Incremental message 2 (no update): prevSeqId = 15, seqId = 15
4. Incremental message 3 (sequence reset): prevSeqId = 15, seqId = 3
5. Incremental message 4 (normal update): prevSeqId = 3, seqId = 5

> Push Data Example of bbo-tbt channel

```json
{
  "arg": {
    "channel": "bbo-tbt",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        [
          "111.06","55154","0","2"
        ]
      ],
      "bids": [
        [
          "111.05","57745","0","2"
        ]
      ],
      "ts": "1670324386802",
      "seqId": 363996337
    }
  ]
}
```

> Push Data Example of bbo-rpi-tbt channel

```json
{
  "arg": {
    "channel": "bbo-rpi-tbt",
    "instId": "BTC-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        [
          "67855.2","0.5","0","1"
        ]
      ],
      "bids": [
        [
          "67858.1","1.2","0","2"
        ]
      ],
      "ts": "1792022400010",
      "seqId": 363996337
    }
  ]
}
```

> Push Data Example of books5 channel

```json
{
  "arg": {
    "channel": "books5",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        ["111.06","55154","0","2"],
        ["111.07","53276","0","2"],
        ["111.08","72435","0","2"],
        ["111.09","70312","0","2"],
        ["111.1","67272","0","2"]],
      "bids": [
        ["111.05","57745","0","2"],
        ["111.04","57109","0","2"],
        ["111.03","69563","0","2"],
        ["111.02","71248","0","2"],
        ["111.01","65090","0","2"]],
      "instId": "BCH-USDT-SWAP",
      "ts": "1670324386802",
      "seqId": 363996337
    }
  ]
}
```
