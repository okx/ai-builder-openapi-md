---
site: my
lang: en
name: WS / Algo orders channel
type: guide
---

### WS / Algo orders channel

Retrieve algo orders (includes `trigger` order, `oco` order, `conditional` order). Data will not be pushed when first subscribed. Data will only be pushed when there are order updates.

#### URL Path

/ws/v5/business (required login)

> Request Example : single

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "orders-algo",
      "instType": "FUTURES",
      "instFamily": "BTC-USD",
      "instId": "BTC-USD-200329"
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
          "channel": "orders-algo",
          "instType": "FUTURES",
          "instFamily": "BTC-USD",
          "instId": "BTC-USD-200329"
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
      "channel": "orders-algo",
      "instType": "FUTURES",
      "instFamily": "BTC-USD"
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
          "channel": "orders-algo",
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

| Parameter        | Type   | Required | Description                                                                       |
|:-----------------|:-------|----------|-----------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op               | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                             |
| args             | Array of objects  | Yes      | List of subscribed channels                                                       |
| > channel        | String | Yes      | Channel name<br>`orders-algo`                                                       |
| > instType       | String | Yes      | Instrument type <br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES` <br>`ANY` |
| > instFamily     | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                             |
| > instId         | String | No       | Instrument ID                                                                     |

> Successful Response Example : single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "orders-algo",
    "instType": "FUTURES",
    "instFamily": "BTC-USD",
    "instId": "BTC-USD-200329"
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
    "channel": "orders-algo",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders-algo\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter        | Type   | Required | Description                                                                       |
|:-----------------|:-------|----------|-----------------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event            | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                          |
| arg              | Object | No       | Subscribed channel                                                                |
| > channel        | String | Yes      | Channel name                                                                      |
| > instType       | String | Yes      | Instrument type <br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES` <br>`ANY` |
| > instFamily     | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                        |
| > instId         | String | No       | Instrument ID                                                                     |
| code             | String | No       | Error code                                                                        |
| msg              | String | No       | Error message                                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: single

```json
{
	"arg": {
		"channel": "orders-algo",
        "uid": "77982378738415879",
		"instType": "FUTURES",
		"instId": "BTC-USD-200329"
	},
	"data": [{
		"actualPx": "0",
		"actualSide": "",
		"actualSz": "0",
		"algoClOrdId": "",
		"algoId": "581878926302093312",
        "attachAlgoOrds": [],
		"amendResult": "",
		"cTime": "1685002746818",
        "uTime": "1708679675245",
		"ccy": "",
		"clOrdId": "",
		"closeFraction": "",
		"failCode": "",
		"instId": "BTC-USDC",
		"instType": "SPOT",
		"last": "26174.8",
		"lever": "0",
		"notionalUsd": "11.0",
		"ordId": "",
		"ordIdList": [],
		"ordPx": "",
		"ordType": "conditional",
		"posSide": "",
		"quickMgnType": "",
		"reduceOnly": "false",
		"reqId": "",
		"side": "buy",
		"slOrdPx": "",
		"slTriggerPx": "",
		"slTriggerPxType": "",
		"state": "live",
		"sz": "11",
		"tag": "",
		"tdMode": "cross",
		"tgtCcy": "quote_ccy",
		"tpOrdPx": "-1",
		"tpTriggerPx": "1",
		"tpTriggerPxType": "last",
		"triggerPx": "",
		"triggerTime": "",
		"tradeQuoteCcy": "USDT",
      	"amendPxOnTriggerType": "0",
        "linkedOrd":{
                "ordId":"98192973880283"
        },
		"isTradeBorrowMode": ""
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter**          | **Type**        | **Description**                                                                                                                                                                                                                                                  |
|:-----------------------|:----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                    | Object          | Successfully subscribed channel                                                                                                                                                                                                                                  |
| > channel              | String          | Channel name                                                                                                                                                                                                                                                     |
| > uid                  | String          | User Identifier                                                                                                                                                                                                                                                  |
| > instType             | String          | Instrument type                                                                                                                                                                                                                                                  |
| > instFamily           | String          | Instrument family                                                                                                                                                                                                                                                |
| > instId               | String          | Instrument ID                                                                                                                                                                                                                                                    |
| data                   | Array of objects           | Subscribed data                                                                                                                                                                                                                                                  |
| > instType             | String          | Instrument type                                                                                                                                                                                                                                                  |
| > instId               | String          | Instrument ID                                                                                                                                                                                                                                                    |
| > ccy                    | String          | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`, `FUTURES` and `SWAP` contracts.                                                                                                                                                                                                                                                   |
| > ordId                | String          | Latest order ID, the order ID associated with the algo order. It will be deprecated soon                                                                                                                                                                                                   |
| > ordIdList            | Array of strings          | Order ID list. There will be multiple order IDs when there is TP/SL splitting order. Empty for a trigger+chase (the spawned order is an algo — see `subAlgoIdList`).                                                                                                                                                                             |
| > subAlgoIdList        | Array of strings          | `algoId`(s) of the algo order(s) spawned when the trigger fires. For `advanceOrdType` `chase`, holds the spawned chase order's `algoId` after the trigger fires; empty before then. Mirrors `ordIdList`, which records spawned regular orders. |
| > algoId               | String          | Algo ID                                                                                                                                                                                                                                                          |
| > clOrdId              | String          | Client Order ID as assigned by the client                                                                                                                                                                                                                        |
| > sz                   | String          | Quantity to buy or sell.<br>`SPOT`/`MARGIN`: in the unit of currency.<br>`FUTURES`/`SWAP`/`OPTION`: in the unit of contract.                                                                                                                                             |
| > ordType              | String          | Order type<br>`conditional`: One-way stop order  <br>`oco`: One-cancels-the-other order <br>`trigger`: Trigger order <br>`chase`: Chase order                                                                                                                                          |
| > side                 | String          | Order side<br>`buy`<br>`sell`                                                                                                                                                                                                                                        |
| > posSide              | String          | Position side <br>`net`<br>`long` or `short`<br>Only applicable to `FUTURES`/`SWAP`                                                                                                                                                                                  |
| > tdMode               | String          | Trade mode<br>`cross`: cross<br>`isolated`: isolated<br>`cash`: cash                                                                                                                                                                                                     |
| > tgtCcy               | String          | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency<br>`quote_ccy`: Quote currency<br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                 |
| > lever                | String          | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                                                     |
| > state                | String          | Order status  <br>`live`: to be effective <br>`effective`: effective <br>`canceled`: canceled <br>`order_failed`: order failed<br>`partially_failed`: partially failed<br>`partially_effective`: partially effective                                                                                           |
| > tpTriggerPx          | String          | Take-profit trigger price.                                                                                                                                                                                                                                       |
| > tpTriggerPxType      | String          | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                             |
| > tpOrdPx              | String          | Take-profit order price.                                                                                                                                                                                                                                         |
| > slTriggerPx          | String          | Stop-loss trigger price.                                                                                                                                                                                                                                         |
| > slTriggerPxType      | String          | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                               |
| > slOrdPx              | String          | Stop-loss order price.                                                                                                                                                                                                                                           |
| > triggerPx            | String          | Trigger price                                                                                                                                                                                                                                                    |
| > triggerPxType        | String          | Trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                         |
| > ordPx                | String          | Order price for the trigger order                                                                                                                                                                                                                                                      |
| > advanceOrdType       | String          | Sub-order type for trigger orders.<br>`fok`: Fill-or-kill order<br>`ioc`: Immediate-or-cancel order<br>`chase`: Chase limit order<br>Default is "". |
| > advChaseParams       | Array of objects | Chase parameters. Present when `advanceOrdType` is `chase`. |
| >> chaseType           | String | Chase distance unit. `distance` or `ratio`. |
| >> chaseVal            | String | Chase value. `0` tracks the best bid/ask directly; `>0` is a distance. |
| >> maxChaseType        | String | Maximum chase distance unit. `distance` or `ratio`. |
| >> maxChaseVal         | String | Maximum chase distance value. |
| > last                 | String          | Last filled price while placing                                                                                                                                                                                                                                  |
| > actualSz             | String          | Actual order quantity                                                                                                                                                                                                                                            |
| > actualPx             | String          | Actual order price                                                                                                                                                                                                                                               |
| > notionalUsd          | String          | Estimated national value in `USD` of order                                                                                                                                                                                                                       |
| > tag                  | String          | Order tag                                                                                                                                                                                                                                                        |
| > actualSide           | String          | Actual trigger side<br/>Only applicable to oco order and conditional order                                                                                                                                                                                                                                              |
| > triggerTime          | String          | Trigger time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                        |
| > reduceOnly           | String          | Whether the order can only reduce the position size. Valid options: `true` or `false`.                                                                                                                                                                           |
| > failCode             | String          | It represents that the reason that algo order fails to trigger. It is "" when the state is `effective`/`canceled`. There will be value when the state is `order_failed`, e.g. 51008;<br>Only applicable to Stop Order, Trailing Stop Order, Trigger order.       |
| > algoClOrdId          | String          | Client Algo Order ID as assigned by the client.                                                                                                                                                                                                                  |
| > reqId                | String          | Client Request ID as assigned by the client for order amendment. "" will be returned if there is no order amendment.                                                                                                                                             |
| > amendResult          | String          | The result of amending the order<br>`-1`: failure <br>`0`: success                                                                                                                                                                                               |
| > amendPxOnTriggerType | String          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                   |
| > attachAlgoOrds       | Array of objects | Attached TP/SL or trailing stop order info<br>Applicable to `Futures mode/Multi-currency margin/Portfolio margin`                                                                                                                                                      |
| >> attachAlgoClOrdId   | String          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing the attached algo order once the general order is filled completely. |
| >> tpTriggerPx         | String          | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                              |
| >> tpTriggerRatio      | String          | Take-profit trigger ratio, 0.3 represents 30%. Only applicable to `FUTURES`/`SWAP` contracts.                                                                            |
| >> tpTriggerPxType     | String          | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                     |
| >> tpOrdPx             | String          | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                  |
| >> slTriggerPx         | String          | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                          |
| >> slTriggerRatio      | String          | Stop-loss trigger ratio, 0.3 represents 30%. Only applicable to `FUTURES`/`SWAP` contracts.                                                                            |
| >> slTriggerPxType     | String          | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                        |
| >> slOrdPx             | String          | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                               |
| >> callbackRatio       | String | Callback ratio, e.g. `0.05` represents 5% |
| >> callbackSpread      | String | Callback spread (price distance) |
| >> activePx            | String | Activation price |
| > linkedOrd | Object | Linked TP order detail, only applicable to SL order that comes from the one-cancels-the-other (OCO) order that contains the TP limit order.                                                                                                                                                                               |
| >> ordId               | String | Order ID                                                                                      |
| > cTime                | String   | Creation time Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                   |
| > uTime                | String            | Order updated time, Unix timestamp format in milliseconds, e.g. 1597026383085                 |
| > isTradeBorrowMode            | String            | Whether borrowing currency automatically<br> true<br> false<br>Only applicable to `trigger order`, `trailing order` and `twap order`                                                                                   |
| > chaseType            | String | Chase type. Only applicable to `chase` order. |
| > chaseVal             | String | Chase value. Only applicable to `chase` order.                                 |
| > maxChaseType         | String | Maximum chase type. Only applicable to `chase` order.                          |
| > maxChaseVal          | String | Maximum chase value. Only applicable to `chase` order.                         |
| > tradeQuoteCcy | String | The quote currency used for trading.|
