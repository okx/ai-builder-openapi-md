---
site: okex
lang: en
name: WS / Grid positions channel
type: guide
---

### WS / Grid positions channel

Retrieve contract grid positions. Data will be pushed when triggered by events such as placing/canceling order.<br>
Please ignore the empty data.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-positions",
		"algoId": "449327675342323712"
	}]
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
    args = [{
		"channel": "grid-positions",
		"algoId": "449327675342323712"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                       |
| :--------- | :----- | -------- | --------------------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                             |
| args     | Array of objects  | Yes      | List of subscribed channels                                                       |
| > channel  | String | Yes      | Channel name<br>`grid-positions`                                                                      |
| > algoId   | String | Yes       | Algo Order ID                                                       |

> Successful Response Example

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-positions",
		"algoId": "449327675342323712"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-positions\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter  | Type   | Required | Description                                                                       |
| :--------- | :----- | -------- | --------------------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                         |
| arg      | Object | No       | Subscribed channel                                                                  |
| > channel  | String | Yes      | Channel name                                                                      |
| > algoId   | String | Yes       | Algo Order ID                                                                     |
| code       | String | No       | Error code                                                                        |
| msg        | String | No       | Error message                                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: 

```json
{
	"arg": {
		"channel": "grid-positions",
		"uid": "4470****9584",
		"algoId": "449327675342323712"
	},
	"data": [{
		"adl": "1",
        "algoClOrdId": "",
		"algoId": "449327675342323712",
		"avgPx": "29181.4638888888888895",
		"cTime": "1653400065917",
		"ccy": "USDT",
		"imr": "2089.2690000000002",
		"instId": "BTC-USDT-SWAP",
		"instType": "SWAP",
		"last": "29852.7",
		"lever": "5",
		"liqPx": "604.7617536513744",
		"markPx": "29849.7",
		"mgnMode": "cross",
		"mgnRatio": "217.71740878394456",
		"mmr": "41.78538",
		"notionalUsd": "10435.794191550001",
		"pTime": "1653536068723",
		"pos": "35",
		"posSide": "net",
		"uTime": "1653445498682",
		"upl": "232.83263888888962",
		"uplRatio": "0.1139826489932205"
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter** | **Type** | **Description**                                                                                                                        |
| :------------ | :------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| arg         | Object   | Successfully subscribed channel                                                                                                        |
| > channel     | String   | Channel name                                                                                                                           |
| > uid    | String   | User Identifier                                                                                                                        |
| > algoId    | String   | Algo Order ID                                                                                                                        |
| data        | Array of objects    | Subscribed data                                                                                                                        |
| > algoId      | String   | Algo ID                        |
| > algoClOrdId         | String   | Client-supplied Algo ID                                                    |
| > instType      | String   | Instrument type |
| > instId       | String   | Instrument ID           |
| > cTime       | String   | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`           |
| > uTime       | String   | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`           |
| > avgPx       | String   | Average open price           |
| > ccy       | String   | Margin currency           |
| > lever       | String   | Leverage           |
| > liqPx       | String   | Estimated liquidation price           |
| > posSide       | String   | Position side<br>`net`          |
| > pos       | String   | Quantity of positions           |
| > mgnMode       | String   | Margin mode<br>`cross`<br>`isolated`       |
| > mgnRatio       | String   | Maintenance margin ratio           |
| > imr       | String   | Initial margin requirement           |
| > mmr       | String   | Maintenance margin requirement           |
| > upl       | String   | Unrealized profit and loss          |
| > uplRatio       | String   | Unrealized profit and loss ratio           |
| > last       | String   | Latest traded price           |
| > notionalUsd       | String   | Notional value of positions in `USD`           |
| > adl       | String   | Automatic-Deleveraging, signal area<br>Divided into 5 levels, from 1 to 5, the smaller the number, the weaker the adl intensity.           |
| > markPx       | String   | Mark price           |
| > pTime       | String   | Push time of positions information, Unix timestamp format in milliseconds, e.g. `1597026383085`          |
