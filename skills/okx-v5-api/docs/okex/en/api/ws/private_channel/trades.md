---
site: okex
lang: en
name: Structure block trades channel
type: guide
---

### Structure block trades channel

Retrieve user's block trades data. All the legs in the same block trade are included in the same update. Data will be pushed whenever there is a block trade that the user is a counterparty for (i.e. the taker or the executing maker). Makers who received a `traded_away` status will not receive data from this channel.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "struc-block-trades"
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
          "channel": "struc-block-trades"
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
| > channel | String | Yes      | Channel name<br>`struc-block-trades`   |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "struc-block-trades"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"struc-block-trades\""}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`struc-block-trades`           |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg":{
        "channel":"struc-block-trades",
        "uid": "77982378738415879"
    },
    "data":[
        {
            "cTime":"1608267227834",
            "rfqId":"18753",
            "clRfqId":"",
            "quoteId":"25092",
            "clQuoteId":"",
            "blockTdId":"180184",
            "tag":"123456",
            "tTraderCode":"ANAND",
            "mTraderCode":"WAGMI",
            "isSuccessful": true,
            "errorCode": "",
            "legs":[
                {
                    "px":"0.0023",
                    "sz":"25.0",
                    "instId":"BTC-USD-20220630-60000-C",
                    "side":"sell",
                    "fee":"0.1001",
                    "feeCcy":"BTC",
                    "tradeId":"10211",
                    "tgtCcy":""

                },
                {
                    "px":"0.0033",
                    "sz":"25",
                    "instId":"BTC-USD-20220630-50000-C",
                    "side":"buy",
                    "fee":"0.1001",
                    "feeCcy":"BTC",
                    "tradeId":"10212",
                    "tgtCcy":""

                }
            ]
        }
    ]
}
```

#### Push data parameters

| **Parameters** | **Types** | **Description**                                                                                                                                                                                       |
| -------------- | --------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg           | Object   | Successfully subscribed channel                                                                                                                                                                       |
| > channel     | String   | Channel name                                                                                                                                                                                          |
| > uid         | String   | User Identifier                                                                                                                                                                                       |
| data          | Array of objects    | Subscribed data                                                                                                                                                                                       |
| > cTime	      | String	 | The time the trade was executed. Unix timestamp in milliseconds.                                                                                                                                      |
| > rfqId	      | String 	 | RFQ ID.                                                                                                                                                                                               |
| > clRfqId	    | String	 | Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker, just return empty string "" for Maker.                                        |
| > quoteId	    | String	 | Quote ID.                                                                                                                                                                                             |
| > clQuoteId	  | String	 | Client-supplied Quote ID. This attribute is treated as client sensitive information. It will not be exposed to the Taker, just return empty string "" for Taker.                                      |
| > blockTdId	  | String	 | Block trade ID.                                                                                                                                                                                       |
| > tag	        | String   | Trade tag. The block trade will have the tag of the RFQ or Quote it corresponds to.                                                                                                                   |
| > tTraderCode	| String 	 | A unique identifier of the Taker. Empty If anonymous mode of RFQ is `True`.                                                                                                                           |
| > mTraderCode	| String	 | A unique identifier of the Maker. Empty If anonymous mode of Quote is `True`.                                                                                                                         |
| > isSuccessful | Boolean | Whether the trade is filled successfully                                                                                                                                                              |
| > errorCode    | String | Error code for unsuccessful trades. <br/>It is "" for successful trade.                                                                                                                               |
| > legs	      | Array of objects| 	Legs of trade                                                                                                                                                                                        |
| >> instId	    | String	 | Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                                                     |
| >> px	        | String	 | The price the leg executed                                                                                                                                                                            |
| >> sz	        | String	 | Size of the leg.                                                                                                                                                                                      |
| >> side	      | String	 | The direction of the leg. Valid value can be buy or sell.                                                                                                                                             |
| >> tgtCcy	    | String	 | Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default. |
| >> fee	      | String	 | Fee. Negative number represents the user transaction fee charged by the platform. Positive fee represents rebate.                                                                                     |
| >> feeCcy	    | String	 | Fee currency                                                                                                                                                                                          |
| >> tradeId	  | String	 | Last traded ID.                                                                                                                                                                                       |
| > acctAlloc | Array of objects | Applicable to both taker, maker |
| >> blockTdId | String | Block trade ID |
| >> errorCode | String | Error code for unsuccessful trades.It is "0" for successful trade. |
| >> acct | String | The name of the allocated account of the RFQOnly applicable to taker, return "" to makers |
| >> legs | Array of objects | The allocated legs of the account. |
| >>> instId | String | The Instrument ID of each leg. Example : "BTC-USDT-SWAP" |
| >>> sz | String | Filled size |
| >>> tradeId | String | Trade ID |
| >>> fee | String | Fee |
| >>> feeCcy | String | Fee currency |

<aside class="notice">
Group RFQ introduction<br><br>

1. This endpoint is at parent RFQ level and contains account allocation. For parent RFQ, we should return the actual executed size, i.e. failed execution size should not be included in the parent RFQ level.<br>
2. For account allocation, we should include both filled and failed child RFQ but add an errorCode to indicate whether a child RFQ is filled.<br>
3. Trade results will only be returned to group RFQ creator. Allocated subaccounts and MSAs will not see trade results. Allocated accounts are expected to get these trades through trading bills.<br>
4. Trades data will only be returned after all child RFQs are execuated.<br>
5. For parent RFQ isSuccessful field,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. it will return true if any child RFQs are filled<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. otherwise, if all child RFQ fails, it will return false<br>
6. Parent RFQ blockTdId or legs tradeId will be empty. However, account allocation breakdown will be offered and tradeId will be attached.<br>
</aside>
