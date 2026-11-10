---
site: okex
lang: en
name: WebSocket Private Channel
type: guide
---

## WebSocket Private Channel

- Production Trading URL: `wss://ws.okx.com/ws/v5/business`
- Demo Trading URL: `wss://wspap.okx.com/ws/v5/business`

### Order channel

Retrieve order information from the `sprd-order` Websocket channel. Data will not be pushed when first subscribed. Data will only be pushed when triggered by events such as placing/canceling order.

#### URL Path

/ws/v5/business (required login)

> Request Example : single

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-orders",
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
          "channel": "sprd-orders",
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
      "channel": "sprd-orders",
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
          "channel": "sprd-orders",
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
|:----------| :--- | :--- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name<br>`sprd-orders` |
| > sprdId  | String | No | Spread ID |

> Successful Response Example : single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-orders",
    "sprdId": "BTC-USDT_BTC-UST-SWAP"
  },
  "connId": "a4d3ae55"
}
```

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-orders"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-orders\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}

```

#### Response parameters

| Parameter | Required | Type | Description |
| :--- | :--- | :--- | --- |
| id           | String   | No       | Unique identifier of the message                        |
| event | Yes | String | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | No | Object | Subscribed channel |
| > channel | Yes | String | Channel name |
| > sprdId | No | String | Spread ID |
| code | No | String | Error code |
| msg | No | String | Error message |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: single

```json
{
  "arg": {
        "channel": "sprd-orders",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP",
        "uid": "614488474791936"
    },
  "data": [
     {
      "sprdId": "BTC-USDT_BTC-UST-SWAP",
      "ordId": "312269865356374016",
      "clOrdId": "b1",
      "tag": "",
      "px": "999",
      "sz": "3",
      "ordType": "limit",
      "side": "buy",
      "fillSz": "0",
      "fillPx": "",
      "tradeId": "",
      "accFillSz": "0",
      "pendingFillSz": "2",
      "pendingSettleSz": "1",
      "canceledSz": "1",
      "state": "live",
      "avgPx": "0",
      "cancelSource": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085",
      "code": "0",
      "msg": "",
      "reqId": "",
      "amendResult": ""
    }
  ]
}
```

#### Push data parameters

| **Parameter**     | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|-------------------| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg               | Object | Successfully subscribed channel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > channel         | String | Channel name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > uid             | String | User Identifier                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > sprdId          | String | spread ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| data              | Array of objects | Subscribed data                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > sprdId          | String | spread ID, e.g.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > ordId           | String | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > clOrdId         | String | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > tag             | String | Order tag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > px              | String | Order price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > sz              | String | The original order quantity, in the unit of szCcy                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > ordType         | String | Order type<br>`market`: Market order <br>`limit`: limit order <br>`post_only`: Post-only order <br>`ioc`: Immediate-or-cancel order                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > side            | String | Order side, buy sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > fillSz          | String | Last trade quantity, only applicable to order updates representing successful settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > fillPx          | String | Last trade price, only applicable to order updates representing successful settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > tradeId         | String | Last trade ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > accFillSz       | String | Accumulated fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > pendingFillSz   | String | Quantity still remaining to be filled                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > pendingSettleSz | String | Quantity that's pending settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > canceledSz      | String | Quantity canceled due order cancellations or trade rejections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > avgPx           | String | Average filled price. If none is filled, it will return "0".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > state           | String | Order state: <br>`canceled` <br>`live` <br>`partially_filled` <br>`filled`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > cancelSource    | String | Source of the order cancellation.Valid values and the corresponding meanings are: <br>`0`: Order canceled by system <br>`1`: Order canceled by user <br>`14`: Order canceled: IOC order was partially canceled due to incompletely filled<br>`15`: Order canceled: The order price is beyond the limit<br> `20`: Cancel all after triggered <br>`31`: The post-only order will take liquidity in maker orders<br> `32`: Self trade prevention <br>`34`: Order failed to settle due to insufficient margin <br>`35`: Order cancellation due to insufficient margin from another order<br>`44`: Your order was canceled because your available balance of this crypto was insufficient for auto conversion. Auto conversion was triggered when the total collateralized liabilities for this crypto reached the platform’s risk control limit. |
| > uTime           | String | Update time, Unix timestamp format in milliseconds, e.g. 1597026383085                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > cTime           | String | Creation time, Unix timestamp format in milliseconds, e.g. 1597026383085                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > code            | String | Error Code, the default is 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > msg             | String | Error Message, the default is ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > reqId           | String | Client Request ID as assigned by the client for order amendment. "" will be returned if there is no order amendment.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > amendResult     | String | The result of amending the order <br>`-1`: failure <br>`0`: success<br>"" will be returned if there is no order amendment.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
