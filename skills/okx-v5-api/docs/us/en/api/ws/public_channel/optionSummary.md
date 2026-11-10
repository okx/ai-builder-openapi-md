---
site: us
lang: en
name: Option summary channel
type: guide
---

### Option summary channel

Retrieve detailed pricing information of all OPTION contracts. Data will be pushed at once.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "opt-summary",
      "instFamily": "BTC-USD"
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
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "opt-summary",
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

| Parameter | Type   | Required | Description                          |
| :-------- | :----- | :------- | ------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args    | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`opt-summary`          |
| > instFamily     | String | Yes      | Instrument family                           |

> Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "opt-summary",
    "instFamily": "BTC-USD"
  },
  "connId": "a4d3ae55"
}
```

> Failure example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"opt-summary\", \"uly\" : \"BTC-USD\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter       | Type   | Required | Description                              |
|:----------------| :----- | :------- | ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event           | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg             | Object | No       | Subscribed channel                       |
| > channel       | String | Yes      | Channel name                             |
| > instFamily    | String | Yes      | Instrument family                               |
| code            | String | No       | Error code                               |
| msg             | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
    "arg": {
        "channel": "opt-summary",
        "instFamily": "BTC-USD"
    },
    "data": [
        {
            "instType": "OPTION",
            "instId": "BTC-USD-241013-70000-P",
            "uly": "BTC-USD",
            "delta": "-1.1180902625",
            "gamma": "2.2361957091",
            "vega": "0.0000000001",
            "theta": "0.0000032334",
            "lever": "8.465747567",
            "markVol": "0.3675503331",
            "bidVol": "0",
            "askVol": "1.1669998535",
            "realVol": "",
            "deltaBS": "-0.9999672034",
            "gammaBS": "0.0000000002",
            "thetaBS": "28.2649858387",
            "vegaBS": "0.0000114332",
            "ts": "1728703155650",
            "fwdPx": "62604.6993093463",
            "volLv": "0.2044711229"
        }
    ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                |
|:--------------| :------- | ------------------------------------------------------------------------------ |
| arg           | Object   | Successfully subscribed channel                                                |
| > channel     | String   | Channel name                                                                   |
| > instFamily  | String   | Instrument family                                                              |
| data          | Array of objects    | Subscribed data                                                                |
| > instType    | String   | Instrument type, `OPTION`                                                      |
| > instId      | String   | Instrument ID                                                                  |
| > uly         | String   | Underlying                                                                     |
| > delta       | String   | Sensitivity of option price to `uly` price                                     |
| > gamma       | String   | The delta is sensitivity to `uly` price                                        |
| > vega        | String   | Sensitivity of option price to implied volatility                              |
| > theta       | String   | Sensitivity of option priceo remaining maturity                                |
| > deltaBS     | String   | Sensitivity of option price to `uly` price in BS mode                          |
| > gammaBS     | String   | The delta is sensitivity to `uly` price in BS mode                             |
| > vegaBS      | String   | Sensitivity of option price to implied volatility in BS mode                   |
| > thetaBS     | String   | Sensitivity of option price to remaining maturity in BS mode                   |
| > lever       | String   | Leverage                                                                       |
| > markVol     | String   | Mark volatility                                                                |
| > bidVol      | String   | Bid volatility                                                                 |
| > askVol      | String   | Ask Volatility                                                                 |
| > realVol     | String   | Realized volatility (not currently used)                                       |
| > volLv       | String | Implied volatility of at-the-money options |
| > fwdPx       | String   | Forward price                       |
| > ts          | String   | Price update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
