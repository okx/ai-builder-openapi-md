---
site: okex
lang: en
name: Trades channel
type: guide
---

### Trades channel

All updates relating to User's Trades are sent through the `sprd-trades` WebSocket Notifications channel.

This is a private channel and consumable solely by the authenticated user.

Updates received through the `sprd-trades` WebSocket Notification channel can include Trades being `filled` or `rejected`.

You may receive multiple notifications if an Order of yours interacts with more than one other Order.

#### URL Path

/ws/v5/business (required login)

> Request Example : single

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-trades",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
          "channel": "sprd-trades",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> Request Example:

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-trades",
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
          "channel": "sprd-trades",
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>`sprd-trades` |
| > sprdId | String | No | Spread ID |

#### Response parameters

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | --- |
| id           | String   | No       | Unique identifier of the message                        |
| event | String | Yes | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name |
| > sprdId | String | No | Spread ID |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```shell
{
    "arg": {
        "channel": "sprd-trades",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP",
        "uid": "614488474791936"
    },
    "data":[
         {
            "sprdId":"BTC-USDT-SWAP_BTC-USDT-200329",
            "tradeId":"123",
            "ordId":"123445",
            "clOrdId": "b16",
            "tag":"",
            "fillPx":"999",
            "fillSz":"3",
            "state": "filled",
            "side":"buy",
            "execType":"M",
            "ts":"1597026383085",
            "legs": [
                {
                    "instId": "BTC-USDT-SWAP",
                    "px": "20000",
                    "sz": "3",
                    "szCont": "0.03",
                    "side": "buy",
                    "fillPnl": "",
                    "fee": "",
                    "feeCcy": "",
                    "tradeId": "1232342342"
                },
                {
                    "instId": "BTC-USDT-200329",
                    "px": "21000",
                    "sz": "3",
                    "szCont": "0.03",
                    "side": "sell",
                    "fillPnl": "",
                    "fee": "",
                    "feeCcy": "",
                    "tradeId": "5345646634"
                },
            ]
            "code": "",
            "msg": ""
        }
    ]
}
```

#### Push Data Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > uid | String | User Identifier |
| > sprdId | String | spread ID |
| data | Array of objects | Subscribed data |
| > sprdId | String | spread ID |
| > tradeId | String | Trade ID |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > tag | String | Order tag |
| > fillPx | String | Last filled price |
| > fillSz | String | Last filled quantity |
| > side | String | Order side,  buy  sell |
| > state | String | Trade state. Valid values are filled and rejected |
| > execType | String | Liquidity taker or maker <br>`T`: taker  <br>`M`: maker |
| >ts | String | Data generation time, Unix timestamp format in milliseconds, e.g. 1597026383085. |
| > legs | Array of objects | Legs of trade |
| >> instId | String | Instrument ID, e.g. BTC-USDT-SWAP |
| >> px | String | The price the leg executed |
| >> sz | String | Size of the leg in contracts or spot. |
| >> szCont | String | Filled amount of the contract  <br>Only applicable to contracts, return "" for spot |
| >> side | String | The direction of the leg. Valid value can be `buy` or `sell`. |
| >> fillPnl | String | Last filled profit and loss, applicable to orders which have a trade and aim to close position. It always is 0 in other conditions                                |
| >> fee | String | Fee. Negative number represents the user transaction fee charged by the platform. Positive number represents rebate. |
| >> feeCcy | String | Fee currency |
| >> tradeId | String | Traded ID in the OKX orderbook. |
| > code | String | Error Code, the default is 0 |
| > msg | String | Error Message, the default is "" |
