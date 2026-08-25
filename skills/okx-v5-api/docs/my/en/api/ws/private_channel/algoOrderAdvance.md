---
site: my
lang: en
name: WS / Advance algo orders channel
type: guide
---

### WS / Advance algo orders channel

Retrieve advance algo orders (including Iceberg order, TWAP order, Trailing order). Data will be pushed when first subscribed. Data will be pushed when triggered by events such as placing/canceling order.

#### URL Path

/ws/v5/business (required login)

> Request Example : single

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "algo-advance",
      "instType": "SPOT",
      "instId": "BTC-USDT"
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
          "channel": "algo-advance",
          "instType": "SPOT",
          "instId": "BTC-USDT"
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
      "channel": "algo-advance",
      "instType": "SPOT"
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
          "channel": "algo-advance",
          "instType": "SPOT"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                       |
|:-----------|:-------|----------|-----------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                             |
| args       | Array of objects  | Yes      | List of subscribed channels                                                       |
| > channel  | String | Yes      | Channel name<br>`algo-advance`                                                      |
| > instType | String | Yes      | Instrument type <br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES` <br>`ANY` |
| > instId   | String | No       | Instrument ID                                                                     |
| > algoId   | String | No       | Algo Order ID                                                                     |

> Successful Response Example : single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "algo-advance",
    "instType": "SPOT",
    "instId": "BTC-USDT"
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
    "channel": "algo-advance",
    "instType": "SPOT"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"algo-advance\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter  | Type   | Required | Description                                                                       |
|:-----------|:-------|----------|-----------------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                          |
| arg        | Object | No       | Subscribed channel                                                                |
| > channel  | String | Yes      | Channel name                                                                      |
| > instType | String | Yes      | Instrument type <br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES` <br>`ANY` |
| > instId   | String | No       | Instrument ID                                                                     |
| > algoId   | String | No       | Algo Order ID                                                                     |
| code       | String | No       | Error code                                                                        |
| msg        | String | No       | Error message                                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: single

```json
{
    "arg":{
        "channel":"algo-advance",
        "uid": "77982378738415879",
        "instType":"SPOT",
        "instId":"BTC-USDT"
    },
    "data":[
        {
            "actualPx":"",
            "actualSide":"",
            "actualSz":"0",
            "algoId":"355056228680335360",
            "cTime":"1630924001545",
            "ccy":"",
            "clOrdId": "",
            "count":"1",
            "instId":"BTC-USDT",
            "instType":"SPOT",
            "lever":"0",
            "notionalUsd":"",
            "ordPx":"",
            "ordType":"iceberg",
            "pTime":"1630924295204",
            "posSide":"net",
            "pxLimit":"10",
            "pxSpread":"1",
            "pxVar":"",
            "side":"buy",
            "slOrdPx":"",
            "slTriggerPx":"",
            "state":"pause",
            "sz":"0.1",
            "szLimit":"0.1",
            "tdMode":"cash",
            "timeInterval":"",
            "tpOrdPx":"",
            "tpTriggerPx":"",
            "tag": "adadadadad",
            "triggerPx":"",
            "triggerTime":"",
            "tradeQuoteCcy": "USDT",
            "callbackRatio":"",
            "callbackSpread":"",
            "activePx":"",
            "moveTriggerPx":"",
          	"failCode": "",
		        "algoClOrdId": "",
            "reduceOnly": "",
            "isTradeBorrowMode": true
        }
    ]
}
```

#### Response parameters when data is pushed.

| **Parameter**    | **Type** | **Description**                                                                                                                                                                                                                                             |
|:-----------------|:---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg              | Object   | Successfully subscribed channel                                                                                                                                                                                                                             |
| > channel        | String   | Channel name                                                                                                                                                                                                                                                |
| > uid            | String   | User Identifier                                                                                                                                                                                                                                             |
| > instType       | String   | Instrument type                                                                                                                                                                                                                                             |
| > instId         | String   | Instrument ID                                                                                                                                                                                                                                               |
| > algoId         | String   | Algo Order ID                                                                                                                                                                                                                                               |
| data             | Array of objects    | Subscribed data                                                                                                                                                                                                                                             |
| > instType       | String   | Instrument type                                                                                                                                                                                                                                             |
| > instId         | String   | Instrument ID                                                                                                                                                                                                                                               |
| > ccy                    | String          | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`, `FUTURES` and `SWAP` contracts.                                                                                                                                                                                                                                                   |
| > ordId          | String   | Order ID, the order ID associated with the algo order.                                                                                                                                                                                                      |
| > algoId         | String   | Algo ID                                                                                                                                                                                                                                                     |
| > clOrdId        | String   | Client Order ID as assigned by the client                                                                                                                                                                                                                   |
| > sz             | String   | Quantity to buy or sell. `SPOT`/`MARGIN`: in the unit of currency. `FUTURES`/`SWAP`/`OPTION`: in the unit of contract.                                                                                                                                            |
| > side           | String   | Order side, `buy` `sell`                                                                                                                                                                                                                                    |
| > posSide        | String   | Position side <br>`net`  <br>`long` or `short` Only applicable to `FUTURES`/`SWAP`                                                                                                                                                                          |
| > tdMode         | String   | Trade mode, `cross`: cross `isolated`: isolated `cash`: cash                                                                                                                                                                                                |
| > tgtCcy         | String   | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                         |
| > lever          | String   | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                                               |
| > state          | String   | Order status  <br>`live`: to be effective <br>`effective`: effective<br>`partially_effective`: partially effective<br>`canceled`: canceled <br>`order_failed`: order failed <br>`pause`: pause                                                                          |
| > tpTriggerPx    | String   | Take-profit trigger price.                                                                                                                                                                                                                                  |
| > tpOrdPx        | String   | Take-profit order price.                                                                                                                                                                                                                                    |
| > slTriggerPx    | String   | Stop-loss trigger price.                                                                                                                                                                                                                                    |
| > slOrdPx        | String   | Stop-loss order price.                                                                                                                                                                                                                                      |
| > triggerPx      | String   | Trigger price                                                                                                                                                                                                                                               |
| > ordPx          | String   | Order price                                                                                                                                                                                                                                                 |
| > actualSz       | String   | Actual order quantity                                                                                                                                                                                                                                       |
| > actualPx       | String   | Actual order price                                                                                                                                                                                                                                          |
| > notionalUsd    | String   | Estimated national value in `USD` of order                                                                                                                                                                                                                    |
| > tag            | String   | Order tag                                                                                                                                                                                                                                                   |
| > actualSide     | String   | Actual trigger side                                                                                                                                                                                                                                         |
| > triggerTime    | String   | Trigger time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                   |
| > cTime          | String   | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                  |
| > pxVar          | String   | Price ratio  <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                        |
| > pxSpread       | String   | Price variance  <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                     |
| > szLimit        | String   | Average amount <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                      |
| > pxLimit        | String   | Price limit <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                         |
| > timeInterval   | String   | Time interval <br>Only applicable to `twap` order                                                                                                                                                                                                          |
| > count          | String   | Algo Order count <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                    |
| > callbackRatio  | String   | Callback price ratio<br>Only applicable to `move_order_stop` order                                                                                                                                                                                          |
| > callbackSpread | String   | Callback price variance<br>Only applicable to `move_order_stop` order                                                                                                                                                                                       |
| > activePx       | String   | Active price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                  |
| > moveTriggerPx  | String   | Trigger price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                 |
| > failCode        | String | It represents that the reason that algo order fails to trigger. It is "" when the state is `effective`/`canceled`. There will be value when the state is `order_failed`, e.g. 51008;<br>Only applicable to Stop Order, Trailing Stop Order, Trigger order. |
| > algoClOrdId     | String | Client Algo Order ID as assigned by the client.                                                                                                                                                                                                             |
| > reduceOnly         | String | Whether the order can only reduce the position size. Valid options: `true` or `false`.                                                                                                                                                                      |
| > pTime          | String   | Push time of algo order information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                                                                             |
| > isTradeBorrowMode            | Boolean            | Whether borrowing currency automatically<br> true<br> false<br>Only applicable to `trigger order`, `trailing order` and `twap order`                                                                                   |
| > tradeQuoteCcy | String | The quote currency used for trading.|
