---
site: us
lang: en
name: Estimated delivery/exercise/settlement price channel
type: guide
---

### Estimated delivery/exercise/settlement price channel

Retrieve the estimated delivery/exercise/settlement price of `FUTURES`, `OPTION` and `SWAP` contracts.

The estimated price, calculated based on index price during the 30-minute period prior to delivery, excerise, or settlement, with updates pushed approximately every 200ms.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "estimated-price",
      "instType": "FUTURES",
      "instFamily": "BTC-USD"
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
          "channel": "estimated-price",
          "instType": "FUTURES",
          "instFamily": "BTC-USD"
        }
    ]
    
    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter    | Type             | Required        | Description                                                       |
|:-------------|:-----------------|:----------------|-------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op           | String           | Yes             | Operation<br>`subscribe`<br>`unsubscribe`<br>                     |
| args         | Array of objects | Yes             | List of subscribed channels                                       |
| > channel    | String           | Yes             | Channel name<br>`estimated-price`                                 |
| > instType   | String           | Yes             | Instrument type<br>`OPTION`<br>`FUTURES`<br>`SWAP`<br>`EVENTS`                          |
| > instFamily | String           | Conditional     | Instrument family<br>Either `instFamily` or `instId` is required. |
| > instId     | String           | Conditional     | Instrument ID<br>Either `instFamily` or `instId` is required.     |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "estimated-price",
    "instType": "FUTURES",
    "instFamily": "BTC-USD"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"estimated-price\", \"instId\" : \"FUTURES\",\"uly\" :\"BTC-USD\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter    | Type     | Required    | Description                                      |
|:-------------|:---------|:------------|--------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event        | String   | Yes         | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg          | Object   | No          | Subscribed channel                               |
| > channel    | String   | Yes         | Channel name                                     |
| > instType   | String   | Yes         | Instrument type<br>`OPTION`<br>`FUTURES`<br>`SWAP`<br>`EVENTS`         |
| > instFamily | String   | Conditional | Instrument family                                |
| > instId     | String   | Conditional | Instrument ID                                    |
| code         | String   | No          | Error code                                       |
| msg          | String   | No          | Error message                                    |
| connId       | String   | Yes         | WebSocket connection ID                          |

> Push Data Example

```json
{
    "arg": {
        "channel": "estimated-price",
        "instType": "FUTURES",
        "instFamily": "XRP-USDT"
    },
    "data": [{
        "instId": "XRP-USDT-250307",
        "instType": "FUTURES",
        "settlePx": "2.4230631578947368",
        "settleType": "settlement",
        "ts": "1741244598708"
    }]
}
```

#### Push data parameters

| **Parameter**    | **Type**         | **Description**                                                                                         |
|:-----------------|:-----------------|---------------------------------------------------------------------------------------------------------|
| arg              | Object           | Successfully subscribed channel                                                                         |
| > channel        | String           | Channel name                                                                                            |
| > instType       | String           | Instrument type<br>`FUTURES`<br>`OPTION`<br>`SWAP`<br>`EVENTS`                                                      |
| > instFamily     | String           | Instrument family                                                                                       |
| > instId         | String           | Instrument ID                                                                                           |
| data             | Array of objects | Subscribed data                                                                                         |
| > instType       | String           | Instrument type                                                                                         |
| > instId         | String           | Instrument ID, e.g. `BTC-USD-170310`                                                                    |
| > settleType     | String           | Type<br>`settlement`: Futures settlement<br>`delivery`: Futures delivery<br>`exercise`: Option exercise |
| > settlePx       | String           | Estimated price                                                                                         |
| > ts             | String           | Data update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                           |
