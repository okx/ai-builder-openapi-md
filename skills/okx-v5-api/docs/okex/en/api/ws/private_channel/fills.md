---
site: okex
lang: en
name: WS / Fills channel
type: guide
---

### WS / Fills channel

Retrieve transaction information. Data will not be pushed when first subscribed. Data will only be pushed when there are order book fill events, where tradeId > 0.<br>

The channel is exclusively available to users with trading fee tier VIP4 or above. Other users will receive error code 64003. For other users, please use [WS / Order channel](/docs-v5/en/#order-book-trading-trade-ws-order-channel). 

For `EVENTS`, only data for the YES side is returned regardless of whether the actual order was placed on YES or NO.

#### URL Path

/ws/v5/private (required login)

> Request Example: single

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "fills",
            "instId": "BTC-USDT-SWAP"
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
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "fills",
            "instId": "BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "fills"
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
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "fills"
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
| --- | --- | --- | --- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`subscribe` `unsubscribe` |
| args | Array of objects | Yes | List of subscribed channels |
| > channel | String | Yes | Channel name `fills` |
| > instId | String | No | Instrument ID |

> Successful Response Example: single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "fills",
    "instId": "BTC-USDT-SWAP"
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
    "channel": "fills"
  },
  "connId": "a4d3ae55"
}

```

#### Response parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id           | String   | No       | Unique identifier of the message                        |
| event | String | Yes | Event<br> `subscribe` `unsubscribe` `error` |
| arg | Object | No | Subscribed channel |
| > channel | String | Yes | Channel name |
| > instId | String | No | Instrument ID |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId | String | Yes | WebSocket connection ID |

> Push Data Example: single

```json
{
    "arg": {
        "channel": "fills",
        "instId": "BTC-USDT-SWAP",
        "uid": "614488474791111"
    },
    "data":[
        {
            "instId": "BTC-USDT-SWAP",
            "fillSz": "100",
            "fillPx": "70000",
            "side": "buy",
            "ts": "1705449605015",
            "ordId": "680800019749904384",
            "clOrdId": "1234567890",
            "tradeId": "12345",
            "execType": "T",
            "count": "10"
        }
    ]
}

```

#### Push data parameters

| Parameter | Type | Description |
| --- | --- | --- |
| arg | Object | Successfully subscribed channel |
| > channel | String | Channel name |
| > uid | String | User Identifier |
| > instId | String | Instrument ID |
| data | Array of objects | Subscribed data |
| > instId | String | Instrument ID |
| > fillSz | String | Filled quantity. If the trade is aggregated, the filled quantity will also be aggregated.  |
| > fillPx | String | Last filled price |
| > side | String | Trade direction<br> `buy` `sell` |
| > ts | String | Filled time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > ordId | String | Order ID |
| > clOrdId     | String |  Client Order ID as assigned by the client  |
| > tradeId | String | The last trade ID in the trades aggregation |
| > execType | String | Liquidity taker or maker, `T`: taker `M`: maker |
| > count | String | The count of trades aggregated |

<aside class="notice"> 
- The channel is exclusively available to users with trading fee tier VIP4 or above. Others will receive error code 64003 when subscribing to it. <br>
- The channel only pushes partial information of the orders channel. Fill events of block trading, nitro spread, liquidation, ADL, and some other non order book events will not be pushed through this channel. Users should also subscribe to the orders channel for order confirmation. <br>
- When a fill event is received by this channel, the account balance, margin, and position information might not have changed yet. <br>
- Taker orders will be aggregated based on different fill prices. When aggregation occurs, the count field indicates the number of orders matched, and the tradeId represents the tradeId of the last trade in the aggregation. Maker orders will not be aggregated. <br>
- The channel returns clOrdId. The field will be returned upon trade execution. Note that the fills channel will only return this field if the user-provided clOrdId conforms to the signed int64 positive integer format (1-9223372036854775807, 2^63-1); if the user does not provide this field or if clOrdId does not meet the format requirements, the field will return "0". The order endpoints and channel will continue to return the user-provided clOrdId as usual. All request and response parameters are of string type.<br>
- In the future, connection limits will be imposed on this channel. The maximum number of connections subscribing to this channel per subaccount will be 20. We recommend users always use this channel within this limit to avoid any impact on their strategies when the limit is enforced. <br>
</aside>
