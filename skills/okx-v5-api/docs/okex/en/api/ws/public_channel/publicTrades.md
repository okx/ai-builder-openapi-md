---
site: okex
lang: en
name: WebSocket Public Channel
type: guide
---

## WebSocket Public Channel

### Public structure block trades channel

Retrieve the recent block trades data in OKX. All the legs in the same block trade are included in the same update. The data will be pushed 15 minutes after the block trade execution.

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "public-struc-block-trades"
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
          "channel": "public-struc-block-trades"
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
| args      | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`public-struc-block-trades`   |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "public-struc-block-trades"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"public-struc-block-trades\""}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`public-struc-block-trades`           |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg":{
        "channel":"public-struc-block-trades"
    },
    "data":[
        {

            "cTime":"1608267227834",
            "blockTdId":"1802896",
            "groupId":"",
            "legs":[
                {
                    "px":"0.323",
                    "sz":"25.0",
                    "instId":"BTC-USD-20220114-13250-C",
                    "side":"sell",
                    "tradeId":"15102"
                },
                {
                    "px":"0.666",
                    "sz":"25",
                    "instId":"BTC-USD-20220114-21125-C",
                    "side":"buy",
                    "tradeId":"15103"
                }
            ]
        }
    ]
}
```

#### Push data parameters

| **Parameters** | **Types** | **Description**                                                                                                   |
|----------------| --------- | ----------------------------------------------------------------------------------------------------------------- |
| arg            | Object   | Successfully subscribed channel                                                                                                        |
| > channel      | String   | Channel name                                                                                                                           |
| data           | Array of objects    | Subscribed data                                                                                                                        |
| > cTime	       | String	 | The time the trade was executed. Unix timestamp in milliseconds.|
| > blockTdId	   | String   | Block trade ID.    |
| > groupId | String | Group RFQ ID<br>Only applicable to group RFQ, return "" for normal RFQ |
| > legs	        | Array of objects|	Legs of trade|
| >> instId	     | String	 | Instrument ID, e.g. BTC-USDT-SWAP|
| >> px	         | String	 | The price the leg executed|
| >> sz          | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| >> side	       | String	 | The direction of the leg from the Takers perspective. Valid value can be `buy` or `sell`.|
| >> tradeId	    | String	 | Last traded ID.|

<aside class="notice">
Group RFQ introduction<br><br>

1. Add new response parameter groupId, facilitating clients to map subaccount execution to group RFQ. Only applicable to group RFQ, return "" for normal RFQ.<br>
2. Data return by this endpoint should be at **parent RFQ level** regardless of the subaccounts allocation. blockTdId and tradeId will be empty.
</aside>

<aside class="notice">
Mapping blockTdId to rfqId<br><br>

For normal RFQs, each <code>blockTdId</code> has a 1:1 relationship with an <code>rfqId</code>. For Group RFQs, one <code>rfqId</code> may correspond to multiple <code>blockTdId</code>s.<br><br>
This channel does not include <code>rfqId</code> directly. Users who are counterparties to the trade (taker and executing maker) can subscribe to the private <a href="/docs-v5/en/#block-trading-websocket-private-channel-structure-block-trades-channel">Structure block trades channel</a>, which provides both <code>rfqId</code> and <code>blockTdId</code>, enabling cross-referencing between the two channels.
</aside>
