---
site: okex
lang: en
name: WS / Grid sub orders channel
type: guide
---

### WS / Grid sub orders channel

Retrieve grid sub orders. Data will be pushed when triggered by events such as placing order.<br>
Please ignore the empty data.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-sub-orders",
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "grid-sub-orders",
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
| > channel  | String | Yes      | Channel name<br>`grid-sub-orders`                                                                      |
| > algoId   | String | Yes       | Algo Order ID                                                       |

> Successful Response Example

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-sub-orders",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-sub-orders\", \"instType\" : \"FUTURES\"}]}",
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
		"channel": "grid-sub-orders",
		"uid": "44705892343619584",
		"algoId": "449327675342323712"
	},
	"data": [{
		"accFillSz": "0",
        "algoClOrdId": "",
		"algoId": "449327675342323712",
		"algoOrdType": "contract_grid",
		"avgPx": "0",
		"cTime": "1653445498664",
		"ctVal": "0.01",
		"fee": "0",
		"feeCcy": "USDT",
		"groupId": "-1",
		"instId": "BTC-USDT-SWAP",
		"instType": "SWAP",
		"lever": "5",
		"ordId": "449518234142904321",
		"ordType": "limit",
		"pTime": "1653486524502",
		"pnl": "",
		"posSide": "net",
		"px": "28007.2",
        "rebate": "0",
        "rebateCcy": "USDT",
		"side": "buy",
		"state": "live",
		"sz": "1",
		"tag":"",
		"tdMode": "cross",
		"uTime": "1653445498674"
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter**           | **Type** | **Description**                                                                                 |
|:------------------------| :------- |-------------------------------------------------------------------------------------------------|
| arg                     | Object   | Successfully subscribed channel                                                                 |
| > channel               | String   | Channel name                                                                                    |
| > uid                   | String   | User Identifier                                                                                 |
| > algoId                | String   | Algo Order ID                                                                                   |
| data                    | Array of objects    | Subscribed data                                                                                 |
| > algoId                | String   | Algo ID                                                                                         |
| > algoClOrdId           | String   | Client-supplied Algo ID                                                    |
| > instType              | String   | Instrument type                                                                                 |
| > instId                | String   | Instrument ID                                                                                   |
| > algoOrdType           | String   | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid |
| > groupId               | String   | Group ID                                                                                        |
| > ordId                 | String   | Sub order ID                                                                                    |
| > cTime                 | String   | Sub order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`             |
| > uTime                 | String   | Sub order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`             |
| > tdMode                | String   | Sub order trade mode<br>Margin mode `cross` `isolated`<br>Non-Margin mode `cash`                |
| > tag                   | String   | Order tag                                                                                       |
| > ordType               | String   | Sub order type<br>`market`: Market order<br>`limit`: Limit order<br>`ioc`: Immediate-or-cancel order                                |
| > sz                    | String   | Sub order quantity to buy or sell                                                               |
| > state                 | String   | Sub order state<br>`canceled`<br>`live`<br>`partially_filled`<br>`filled`<br>`cancelling`       |
| > side                  | String   | Sub order side<br>`buy` `sell`                                                                  |
| > px                    | String   | Sub order price                                                                                 |
| > fee                   | String   | Sub order fee amount                                                                            |
| > feeCcy                | String   | Sub order fee currency                                                                          |
| > rebate                | String   | Sub order rebate amount                                                                         |
| > rebateCcy             | String   | Sub order rebate currency                                                                       |
| > avgPx                 | String   | Sub order average filled price                                                                  |
| > accFillSz             | String   | Sub order accumulated fill quantity                                                             |
| > posSide               | String   | Sub order position side<br>`net`                                                  |
| > pnl                   | String   | Sub order profit and loss                                                                       |
| > ctVal                 | String   | Contract value<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                      |
| > lever                 | String   | Leverage                                                                                        |
| > pTime                 | String   | Push time of orders information, Unix timestamp format in milliseconds, e.g. `1597026383085`    |
