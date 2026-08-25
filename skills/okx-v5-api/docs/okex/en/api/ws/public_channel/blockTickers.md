---
site: okex
lang: en
name: Block tickers channel
type: guide
---

### Block tickers channel

Retrieve the latest block trading volume in the last 24 hours.

The data will be pushed  when triggered by transaction execution event. In addition, it will also be pushed in 5 minutes interval according to subscription granularity.

#### URL Path

/ws/v5/business

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "block-tickers",
        "instId": "BTC-USDT"
    }]
}
```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [{
        "channel": "block-tickers",
        "instId": "BTC-USDT"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters

| Parameter | Type   | Required | Description                          |
| :-------- | :----- | :------- |--------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args    | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`block-tickers`              |
| > instId  | String | Yes      | Instrument ID e.g. BTC-USDT-SWAP     |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "block-tickers",
    "instId": "LTC-USD-200327"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"block-tickers\", \"instId\" : \"LTC-USD-200327\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
| :-------- | :----- | :------- |------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                       |
| > channel | String | Yes      | Channel name<br>`block-tickers`                           |
| > instId  | String | Yes      | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg": {
        "channel": "block-tickers"
    },
    "data": [
        {
            "instType": "SWAP",
            "instId": "LTC-USD-SWAP",
            "volCcy24h": "0",
            "vol24h": "0",
            "ts": "1597026383085"
        }
    ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                          |
| ------------- | -------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg         | Object   | Successfully subscribed channel                                                                                                                                                                          |
| > channel     | String   | Channel name                                                                                                                                                                                             |
| > instId      | String   | Instrument ID                                                                                                                                                                                            |
| data        | Array of objects    | Subscribed data                                                                                                                                                                                          |
| > instId      | String   | Instrument ID                                                                                                                                                                                            |
| > instType    | String   | Instrument type                                                                                                                                                                                          |
| > volCcy24h   | String   | 24h trading volume, with a unit of `currency`. <br>If it is a `derivatives` contract, the value is the number of base currency. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in quote currency. |
| > vol24h      | String   | 24h trading volume, with a unit of `contract`. <br>If it is a `derivatives` contract, the value is the number of contracts. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in base currency.       |
| > ts          | String   | Block ticker data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                           |
