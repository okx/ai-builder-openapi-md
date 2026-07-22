---
site: okex
lang: en
name: WS / Lead trading notification channel
type: guide
---

### WS / Lead trading notification channel

The notification when failing to lead trade.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "copytrading-lead-notification",
		"instType": "SWAP"
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
		"channel": "copytrading-lead-notification",
		"instType": "SWAP"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter  | Type   | Required | Description      |
| :--------- | :----- | -------- |--------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                    |
| args     | Array of objects  | Yes      | List of subscribed channels                              |
| > channel  | String | Yes      | Channel name<br>`copytrading-lead-notification`                                   |
| > instType | String | Yes      | Instrument type<br>`SWAP` |
| > instId   | String | No       | Instrument ID    |

> Successful Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "copytrading-lead-notification",
        "instType": "SWAP"
    },
    "connId": "aa993428"
}
```

> Failure Response Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"copytrading-lead-notification\", \"instType\" : \"FUTURES\"}]}",
  "connId":"a4d3ae55"
}
```

#### Response parameters

| Parameter        | Type   | Required | Description       |
|:-----------------| :----- | -------- | ------------------------------------------------------------------------------------------------ |
| id           | String   | No       | Unique identifier of the message                        |
| event            | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                 |
| arg              | Object | No       | Subscribed channel|
| > channel        | String | Yes      | Channel name      |
| > instType       | String | Yes      | Instrument type<br>`SWAP` |
| > instId         | String | No       | Instrument ID     |
| code             | String | No       | Error code        |
| msg              | String | No       | Error message     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: 

```json
{
    "arg": {
        "channel": "copytrading-lead-notification",
        "instType": "SWAP",
        "uid": "525627088439549953"
    },
    "data": [
        {
            "infoType": "2",
            "instId": "",
            "instType": "SWAP",
            "maxLeadTraderNum": "3",
            "minLeadEq": "",
            "posSide": "",
            "side": "",
            "subPosId": "667695035433385984",
            "uniqueCode": "3AF72F63E3EAD701"
        }
    ]
}
```

#### Push data parameters

| Parameter               | Type   | Description                                                                                                                                                                                                                                                                     |
|:------------------------| :----- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                     | Object | Successfully subscribed channel                                                                                                                                                                                                                                                 |
| > channel               | String | Channel name                                                                                                                                                                                                                                                                    |
| > uid                   | String | User Identifier                                                                                                                                                                                                                                                                 |
| > instType              | String | Instrument type                                                                                                                                                                                                                                                                 |
| data                    | Array of objects  | Subscribed data                                                                                                                                                                                                                                                                 |
| > instType              | String | Instrument type                                                                                                                                                                                                                                                                 |
| > infoType                | String | Information type<br>`1`: lead trading failed due to touch max position limitation  <br>`2`: lead trading failed due to touch the maximum daily number of lead trading <br>`3`: lead trading failed due to your USDT equity less than the minimum USDT equity of lead trading |
| > subPosId                   | String | Lead position ID                                                                                                                                                                                                                                                                |
| > uniqueCode                 | String | Lead trader unique code                                                                                                                                                                                                                                                         |
| > instId               | String | Instrument ID                                                                                                                                                                                                                                                                   |
| > side                 | String | Side `buy` `sell`                                                                                                                                                                                                                                                               |
| > posSide                | String | Position side <br>`long`<br>`short`<br>`net`                                                                                                                                                                                                                              |
| > maxLeadTraderNum               | String | Maximum daily number of lead trading.                                                                                                                                                                                                                                           |
| > minLeadEq               | String | Minimum USDT equity of lead trading.                                                                                                                                                                                                                                            |
