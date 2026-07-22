---
site: us
lang: en
name: Funding rate channel
type: guide
---

### Funding rate channel

Retrieve funding rate. Data will be pushed in 30s to 90s.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "funding-rate",
      "instId": "BTC-USD-SWAP"
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
          "channel": "funding-rate",
          "instId": "BTC-USD-SWAP"
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
| > channel | String | Yes      | Channel name<br>`funding-rate`         |
| > instId  | String | Yes      | Instrument ID                        |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "funding-rate",
    "instId": "BTC-USD-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"funding-rate\", \"instId\" : \"BTC-USD-SWAP\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                              |
| :-------- | :----- | :------- | ---------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | No       | Subscribed channel                       |
| > channel | String | yes      | Channel name                             |
| > instId  | String | No       | Instrument ID                            |
| code      | String | No       | Error code                               |
| msg       | String | No       | Error message                            |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
   "arg":{
      "channel":"funding-rate",
      "instId":"BTC-USD-SWAP"
   },
   "data":[
      {
         "formulaType": "noRate",
         "fundingRate":"0.0001875391284828",
         "fundingTime":"1700726400000",
         "impactValue": "",
         "instId":"BTC-USD-SWAP",
         "instType":"SWAP",
         "interestRate": "",
         "method": "current_period",
         "maxFundingRate":"0.00375",
         "minFundingRate":"-0.00375",
         "nextFundingRate":"",
         "nextFundingTime":"1700755200000",
         "premium": "0.0001233824646391",
         "settFundingRate":"0.0001699799259033",
         "settState":"settled",
         "ts":"1700724675402"
      }
   ]
}
```

#### Push data parameters

| **Parameter**     | **Type** | **Description**                                                                                     |
|:------------------| :------- | --------------------------------------------------------------------------------------------------- |
| arg               | Object   | Successfully subscribed channel                                                                     |
| > channel         | String   | Channel name                                                                                        |
| > instId          | String   | Instrument ID                                                                                       |
| data              | Array of objects    | Subscribed data                                                                                     |
| > instType        | String   | Instrument type<br>`SWAP`: Perpetual futures<br>`FUTURES`: X-Perps futures |
| > instId          | String   | Instrument ID, e.g. `BTC-USD-SWAP`                                                                  |
| > method          | String | Funding rate mechanism <br>`current_period` ~~<br>`next_period`~~(no longer supported) |
| > formulaType     | String | Formula type<br>`noRate`: old funding rate formula<br>`withRate`: new funding rate formula |
| > fundingRate     | String   | Current funding rate                                                                                |
| > nextFundingRate | String | ~~Forecasted funding rate for the next period <br>The nextFundingRate will be "" if the method is `current_period`~~(no longer supported) |
| > fundingTime     | String   | Settlement time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > nextFundingTime | String   | Forecasted funding time for the next period, Unix timestamp format in milliseconds, e.g. `1597026383085`|
| > minFundingRate  | String | The lower limit of the predicted funding rate of the next cycle |
| > maxFundingRate  | String | The upper limit of the predicted funding rate of the next cycle |
| > interestRate    | String | Interest rate |
| > impactValue     | String | Depth weighted amount (in the unit of quote currency) |
| > settState       | String | Settlement state of funding rate <br>`processing` <br>`settled` |
| > settFundingRate | String | If settState = `processing`, it is the funding rate that is being used for current settlement cycle. <br>If settState = `settled`, it is the funding rate that is being used for previous settlement cycle |
| > premium | String | Premium index<br> formula: [Max (0, Impact bid price – Index price) – Max (0, Index price – Impact ask price)] / Index price |
| > ts              | String | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085` |

<aside class="notice">
For some altcoins perpetual swaps with significant fluctuations in funding rates, OKX will closely monitor market changes. When necessary, the funding rate collection frequency, currently set at 8 hours, may be adjusted to higher frequencies such as 6 hours, 4 hours, 2 hours, or 1 hour. Thus, users should focus on the difference between `fundingTime` and `nextFundingTime` fields to determine the funding fee interval of a contract.
</aside>
