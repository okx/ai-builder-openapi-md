---
site: my
lang: en
name: Position risk warning
type: guide
---

### Position risk warning

This push channel is only used as a risk warning, and is not recommended as a risk judgment for strategic trading <br>
In the case that the market is volatile, there may be the possibility that the position has been liquidated at the same time that this message is pushed.<br>
The warning is sent when a position is at risk of liquidation for isolated margin positions. The warning is sent when all the positions are at risk of liquidation for cross-margin positions.<br>
Concurrent connection to this channel will be restricted by the following rules: [WebSocket connection count limit] (/docs-v5/en/#overview-websocket-connection-count-limit).

#### URL Path

/ws/v5/private (required login)

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "liquidation-warning",
      "instType": "ANY"
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
        url = "wss://ws.okx.com:8443/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "liquidation-warning",
          "instType": "ANY"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter        | Type   | Required | Description         |
|:-----------------| :----- | -------- |------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op               | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                              |
| args             | Array of objects  | Yes      | List of subscribed channels                                                        |
| > channel        | String | Yes      | Channel name<br>`liquidation-warning`                                                |
| > instType       | String | Yes      | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`  <br>`ANY` |
| > instFamily     | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                              |
| > instId         | String | No       | Instrument ID       |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "liquidation-warning",
    "instType": "ANY"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"liquidation-warning\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter        | Type   | Required | Description          |
|:-----------------| :----- | -------- | ----------------------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event            | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                           |
| arg              | Object | No       | Subscribed channel   |
| > channel        | String | Yes      | Channel name<br>`liquidation-warning`         |
| > instType       | String | Yes      | Instrument type<br>`OPTION`<br>`FUTURES`<br>`SWAP`<br>`MARGIN`  <br>`ANY` |
| > instFamily     | String | No       | Instrument family           |
| > instId         | String | No       | Instrument ID        |
| code             | String | No       | Error code           |
| msg              | String | No       | Error message        |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg":{
        "channel":"liquidation-warning",
        "uid": "77982378738415879",
        "instType":"FUTURES"
    },
    "data":[
        {
            "cTime":"1619507758793",
            "ccy":"ETH",
            "instId":"ETH-USD-210430",
            "instType":"FUTURES",
            "lever":"10",
            "markPx":"2353.849",
            "mgnMode":"isolated",
            "mgnRatio":"11.731726509588816",
            "pTime":"1619507761462",
            "pos":"1",
            "posCcy":"",
            "posId":"307173036051017730",
            "posSide":"long",
            "uTime":"1619507761462",
        }
    ]
}
```

#### Push data parameters

| Parameter           | Type   | Description                                                             |
|:--------------------| :----- | ----------------------------------------------------------------------- |
| arg                 | Object | Successfully subscribed channel                                         |
| > channel           | String | Channel name                                                            |
| > uid               | String | User Identifier                                                         |
| > instType          | String | Instrument type                                                         |
| > instFamily        | String | Instrument family                                                              |
| > instId            | String | Instrument ID                                                           |
| data                | Array of objects  | Subscribed data                                                         |
| > instType          | String | Instrument type                                                         |
| > mgnMode           | String | Margin mode, `cross` `isolated`                                         |
| > posId             | String | Position ID                                                    |
| > posSide           | String | Position side<br>`long` <br>`short` <br>`net` (`FUTURES`/`SWAP`/`OPTION`: positive `pos` means long position and negative `pos` means short position. `MARGIN`: `posCcy` being base currency means long position, `posCcy` being quote currency means short position.) |
| > pos               | String | Quantity of positions                                                   |
| > posCcy            | String | Position currency, only applicable to `MARGIN` positions                |
| > instId            | String | Instrument ID, e.g. `BTC-USDT-SWAP`                                    |
| > lever             | String | Leverage, not applicable to `OPTION` seller                             |
| > markPx            | String   | Mark price                                                       |
| > mgnRatio          | String | Maintenance margin ratio                                                            |
| > ccy               | String | Currency used for margin                                                |
| > cTime             | String | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                                            |
| > uTime             | String | Latest time position was adjusted, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                        |
| > pTime             | String | Push time of positions information, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                       |

<aside class="notice"> 
Trigger push logic: the trigger logic of the liquidation warning and the liquidation message is the same
</aside>
