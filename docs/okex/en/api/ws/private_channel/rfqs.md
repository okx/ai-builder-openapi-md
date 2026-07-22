---
site: okex
lang: en
name: WebSocket Private Channel
type: guide
---

## WebSocket Private Channel

### Rfqs channel

Retrieve the RFQs sent or received by the user. Data will be pushed whenever the user sends or receives an RFQ.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "rfqs"
    }
  ]
}
```

```python
import asyncio

from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():

    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "rfqs"
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
| > channel | String | Yes      | Channel name<br>`rfqs`              |

> Successful Response Example 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "rfqs"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"rfqs\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`rfqs`                         |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg":{
        "channel":"rfqs",
        "uid": "77982378738415879"
    },
    "data":[
        {
            "cTime":"1611033737572",
            "uTime":"1611033737572",
            "traderCode":"DSK2",
            "rfqId":"22534",
            "clRfqId":"",
            "tag":"123456",
            "state":"active",
            "flowType":"",
            "validUntil":"1611033857557",
            "allowPartialExecution": false,
            "counterparties":[
                "DSK4",
                "DSK5"
            ],
            "legs":[
                {
                    "instId":"BTCUSD-211208-36000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"25.0",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""
                },
                {
                    "instId":"ETHUSD-211208-45000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"25.0",
                    "side":"sell",
                    "posSide": "long",
                    "tgtCcy":""
                }
            ]
        }
    ]
}
```

#### Push data parameters

| **Parameters** | **Types** | **Description**                                                                                                                                                                                                                                                                                                                                              |
| -------------- | --------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg           | Object   | Successfully subscribed channel                                                                                                                                                                                                                                                                                                                              |
| > channel     | String   | Channel name                                                                                                                                                                                                                                                                                                                                                 |
| > uid         | String   | User Identifier                                                                                                                                                                                                                                                                                                                                              |
| data          | Array of objects    | Subscribed data                                                                                                                                                                                                                                                                                                                                              |
| > cTime	      | String   | The timestamp the RFQ was created, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                    |
| > uTime	      | String	 | The timestamp the RFQ was updated latest, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                             |
| > state	      | String	 | The status of the RFQ. Valid values can be `active`, `canceled`, `filled`, `expired` `traded_away` or `failed`. <br/>`filled` indicates the RFQ was successfully executed against the maker's quote. <br/>`traded_away` only applies to Maker. The same RFQ can appear as `filled` to one maker and `traded_away` to another. <br/>Example: taker creates RFQ → makerA quotes pxA, makerB quotes pxB → pxA is better than pxB → taker executes quoteA → makerA sees `filled`, makerB sees `traded_away`.                                                                                                                                                                                                     |
| > counterparties	|Array of Strings| 	The list of counterparties traderCode the RFQ was broadcasted to.                                                                                                                                                                                                                                                                                           |
| > validUntil	| String   | The timestamp the RFQ expires. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                        |
| > clRfqId	    | String	 | Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker. Return empty for Maker, eg. "".                                                                                                                                                                                                      |
| > tag	        | String   | RFQ tag. The block trade associated with the RFQ will have the same tag.                                                                                                                                                                                                                                                                                     |
| > flowType | String | Identify the type of the RFQ. <br>Only applicable to Makers, return "" for Takers                                                                                                                                                                                                                                                                            |
| > traderCode	| String	 | A unique identifier of taker. Empty If anonymous mode is `True`.                                                                                                                                                                                                                                                                                             |
| > rfqId	      | String	 | RFQ ID                                                                                                                                                                                                                                                                                                                                                       |
| > allowPartialExecution	 | Boolean	 | Whether the RFQ can be partially filled provided that the shape of legs stays the same. <br>Valid value is `true` or `false`. <br>`false` by default.                                                                                                                                                                                                        |
| > legs	      | Array of objects| 	An Array of objects containing each leg of the RFQ.                                                                                                                                                                                                                                                                                                         |
| >> instId	    | String	 | Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                                                                                                                                                                                                            |
| >> tdMode	    | String   | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                                          |
| >> ccy   	    | String   | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                                                |
| >> sz	        | String	 | Size of the leg.                                                                                                                                                                                                                                                                                                                                             |
| >> side	      | String	 | The direction of the leg. Valid values can be buy or sell.                                                                                                                                                                                                                                                                                                   |
| >> posSide    | String   | Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to `FUTURES`/`SWAP`. |
| >> tgtCcy	    | String	 | Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default.                                                                                                                                                        |
| >> tradeQuoteCcy	        | String           | 	The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
| > groupId | String | Group RFQ ID<br> Only applicable to group RFQ, return "" for normal RFQ |
| > acctAlloc | Array of objects | Account level allocation of the RFQ<br> This is only applicable to the taker. |
| >> acct | String | The name of the allocated account of the RFQ. |
| >> legs | Array of objects | The allocated legs of the account. |
| >>> instId | String | The Instrument ID of each leg. Example : "BTC-USDT-SWAP" |
| >>> sz | String | The allocated size of each leg. |
| >>> tdMode | String | Trade mode |
| >>> ccy | String | Margin currency |
| >>> posSide | String | Position side |

<aside class="notice">
state: pending_fill is a kind of moment state, and this channel doesn't update it.
</aside>

<aside class="notice">
Group RFQ introduction<br><br>

1. allowPartialExecution field is always true for group RFQ for taker and maker.<br>
2. Add a new response parameter acctAlloc with all account allocation the same as the initial request, but it is only applicable to takers.<br>
3. Add a new response parameter groupId, applicable to both takers and makers.<br>
4. For group RFQ state,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. if any allocated account is pending execution, then pending_fill<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. otherwise,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. if any allocated account is filled, then filled<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. if all allocated accounts are failed, then failed<br>
</aside>
