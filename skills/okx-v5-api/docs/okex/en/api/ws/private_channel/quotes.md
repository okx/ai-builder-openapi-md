---
site: okex
lang: en
name: Quotes channel
type: guide
---

### Quotes channel

Retrieve the Quotes sent or received by the user. Data will be pushed whenever the user sends or receives a Quote.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "quotes"
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "quotes"
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
| > channel | String | Yes      | Channel name<br>`quotes`               |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "quotes"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"quotes\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`quotes`                       |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg":{
        "channel":"quotes",
        "uid": "77982378738415879"
    },
    "data":[
        {
            "validUntil":"1608997227854",
            "uTime":"1608267227834",
            "cTime":"1608267227834",
            "legs":[
                {
                    "px":"0.0023",
                    "sz":"25.0",
                    "instId":"BTC-USD-220114-25000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"sell",
                    "posSide": "long",
                    "tgtCcy":""

                },
                {
                    "px":"0.0045",
                    "sz":"25",
                    "instId":"BTC-USD-220114-35000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""

                }
            ],
            "quoteId":"25092",
            "rfqId":"18753",
            "tag":"123456",
            "traderCode":"SATS",
            "quoteSide":"sell",
            "state":"canceled",
            "reason":"mmp_canceled",
            "clQuoteId":""
        }
    ]
}
```

#### Push data parameters

| **Parameters** | **Types** | **Description**                                                                                                                                                                         |
|----------------| --------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg            | Object   | Successfully subscribed channel                                                                                                                                                         |
| > channel      | String   | Channel name                                                                                                                                                                            |
| > uid          | String   | User Identifier                                                                                                                                                                         |
| data           | Array of objects    | Subscribed data                                                                                                                                                                         |
| > cTime	       | String	 | The timestamp the Quote was created, Unix timestamp format in milliseconds.                                                                                                             |
| > uTime	       | String	 | The timestamp the Quote was updated latest, Unix timestamp format in milliseconds.                                                                                                      |
| > state	       |String	| The status of the quote. Valid values can be `active` `canceled` `filled` `expired` or `failed`.                                                                         |
| > reason	       |String	| Reasons of state. Valid values can be mmp_canceled.                                                     |
| > validUntil	  | String	 | The timestamp the Quote expires. Unix timestamp format in milliseconds.                                                                                                                    |
| > rfqId	       | String	 | RFQ ID.                                                                                                                             |
| > clRfqId	     | String	 | Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker, just return empty string "" for Maker.                           |
| > quoteId	     | String	 | Quote ID                                                                                                                           |
| > clQuoteId	   | String	 | Client-supplied Quote ID. This attribute is treated as client sensitive information. It will not be exposed to the Taker, just return empty string "" for Taker.                         |
| > tag	         | String  | Quote tag. The block trade associated with the Quote will have the same tag.                              |
| > traderCode	  | String	 | A unique identifier of maker. Empty If anonymous mode of Quote is `True`.                                                                                                                 |
| > quoteSide	   | String	 | Top level side of Quote. Its value can be buy or sell.                                                                                                                                  |
| > legs	        | Array of objects| 	The legs of the Quote.                                                                                                                                                                 |
| >> instId	     | String	 | The instrument name of quoted leg.                                                                                                                                                      |
| >> tdMode	     | String  | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                 |
| >> ccy   	     | String  | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                             |
| >> sz	         | String	 | The size of the quoted leg in contracts or spot.                                                                                                                                        |
| >> px	         | String	 | The price of the leg.                                                                                                                                                                   |
| >> side	       | String	 | The direction of the leg. Valid values can be buy or sell.                                                                                                                              |
| >> posSide     | String  | Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to `FUTURES`/`SWAP`. |
| >> tgtCcy	     | String	 | Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default. |
| >> tradeQuoteCcy	        | String           | 	The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
