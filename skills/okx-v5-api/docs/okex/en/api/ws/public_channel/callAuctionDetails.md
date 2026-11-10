---
site: okex
lang: en
name: WS / Call auction details channel
type: guide
---

### WS / Call auction details channel

Retrieve call auction details.

#### URL Path

/ws/v5/public

> Request Example
> 

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "call-auction-details",
        "instId": "ONDO-USDC"
    }]
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
    args = [{
        "channel": "call-auction-details",
        "instId": "ONDO-USDC"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                  |
|:----------| :----- | :------- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`                                                                                                                                                                                                                                                                                                                                   |
| args      | Array of objects  | Yes      | List of subscribed channels                                                                                                                                                                                                                                                                                                                                                  |
| > channel   | String | Yes      | Channel name <br>`call-auction-details` |
| > instId    | String | Yes      | Instrument ID                                                                                                                                                                                                                                                                                                                                                                |

> Successful Response Example
> 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "call-auction-details",
      "instId": "ONDO-USDC"
    },
  "connId": "a4d3ae55"
}

```

> Failure Response Example
> 

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"call-auction-details\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}

```

#### Response parameters

| Parameter | Type     | Required | Description                                      |
|:----------|:---------|:---------|--------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String   | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | No       | Subscribed channel                               |
| > channel   | String   | yes      | channel name                                     |
| > instId    | String   | Yes      | Instrument ID                                    |
| code      | String   | No       | Error code                                       |
| msg       | String   | No       | Error message                                    |
| connId    | String   | Yes      | WebSocket connection ID                          |

> Push Data Example
> 

```json
{
  "arg": {
    "channel": "call-auction-details",
    "instId": "ONDO-USDC"
  },
  "data": [
        {
            "instId": "ONDO-USDC",
            "unmatchedSz": "9988764",
            "eqPx": "0.6",
            "matchedSz": "44978",
            "state": "continuous_trading",
            "auctionEndTime": "1726542000000",
            "ts": "1726542000007"
        }
  ]
}

```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                    |
|-----------| --- | --- |
| arg           | Object   | Successfully subscribed channel                                                                                                                                                                    |
| > channel     | String   | Channel name                                                                                                                                                                                       |
| > instId      | String   | Instrument ID                                                                                                                                                                                      |
| data          | Array of objects    | Subscribed data                                                                                                                                                                                    |
| > instId | String | Instrument ID |
| > eqPx | String | Equilibrium price |
| > matchedSz | String | Matched size for both buy and sell<br>The unit is in base currency |
| > unmatchedSz | String | Unmatched size |
| > auctionEndTime | String | Call auction end time. Unix timestamp in milliseconds. |
| > state | String | Trading state of the symbol<br>`call_auction`<br>`continuous_trading`  |
| > ts | String | Data generation time. Unix timestamp in millieseconds. |

<aside class="notice">
During call auction, users can get the updates of equilibrium price, matched size, unmatched size, and auction end time. The data will be updated around once a second. When call auction ends, this channel will push the last message, returning the actual open price, matched size, and unmatched size, with trading state as `continuous_trading`.
</aside>
