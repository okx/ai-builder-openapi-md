---
site: us
lang: en
name: Price limit channel
type: guide
---

### Price limit channel

Retrieve the maximum buy price and minimum sell price of instruments. Data will be pushed every 200ms  when there are changes in limits, and will not be pushed when there is no changes on limit.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "price-limit",
      "instId": "LTC-USD-190628"
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
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "price-limit",
          "instId": "LTC-USD-190628"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                             |
| :-------- | :----- | :------- |---------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                   |
| args    | Array of objects  | Yes      | List of subscribed channels                             |
| > channel | String | Yes      | Channel name<br>`price-limit`                             |
| > instId | String | Yes | Instrument ID |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "price-limit",
    "instId": "LTC-USD-190628"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"price-limit\", \"instId\" : \"LTC-USD-190628\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
| :-------- | :----- | :------- | ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                       |
| > channel | String | Yes      | Channel name                             |
| > instId  | String | Yes      | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
	"arg": {
		"channel": "price-limit",
		"instId": "LTC-USD-190628"
	},
	"data": [{
		"instId": "LTC-USD-190628",
		"buyLmt": "200",
		"sellLmt": "300",
		"ts": "1597026383085",
        "enabled": true
	}]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                |
| :------------ | :------- | ------------------------------------------------------------------------------ |
| arg         | Object   | Successfully subscribed channel                                                |
| > channel     | String   | Channel name                                                                   |
| > instId      | String   | Instrument ID                                                                  |
| data        | Array of objects    | Subscribed data                                                                |
| > instType    | String   | Instrument type                                                                |
| > instId      | String   | Instrument ID, e.g. `BTC-USDT`                                             |
| > buyLmt | String | Maximum buy price <br>Return "" when enabled is false |
| > sellLmt | String | Minimum sell price <br>Return "" when enabled is false |
| > ts | String | Price update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > enabled | Boolean | Whether price limit is effective <br>`true`: the price limit is effective <br>`false`: the price limit is not effective |
