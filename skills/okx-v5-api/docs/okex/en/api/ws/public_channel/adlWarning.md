---
site: okex
lang: en
name: ADL warning channel
type: guide
---

### ADL warning channel

Auto-deleveraging warning channel.

Data is only pushed in the `warning` or `adl` state, once every second, displaying the security fund balance and related risk information. No data is pushed in the `normal` state.

For more ADL details, please refer to [Introduction to Auto-deleveraging] (https://www.okx.com/help/iv-introduction-to-auto-deleveraging-adl)

#### URL Path

/ws/v5/public

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "adl-warning",
        "instType": "FUTURES",
        "instFamily": "BTC-USDT"
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
        "channel": "adl-warning",
        "instType": "FUTURES",
        "instFamily": "BTC-USDT"
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
| > channel | String | Yes | Channel name<br>`adl-warning` |
| > instType | String | Yes | Instrument type<br>`SWAP`<br>`FUTURES`<br>`OPTION` |
| > instFamily | String | No | Instrument family |

> Successful Response Example

```json
{
   "id": "1512",
   "event":"subscribe",
   "arg":{
      "channel":"adl-warning",
      "instType":"FUTURES",
      "instFamily":"BTC-USDT"
   },
   "connId":"48d8960a"
}

```

> Failure Response Example

```json
{
   "id": "1512",
   "event":"error",
   "msg":"Illegal request: { \"event\": \"subscribe\", \"arg\": { \"channel\": \"adl-warning\", \"instType\": \"FUTURES\", \"instFamily\": \"BTC-USDT\" } }",
   "code":"60012",
   "connId":"48d8960a"
}

```

#### Response parameters

| Parameter  | Type   | Required | Description                                                             |
| :--------- | :----- | :------- | :---------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                |
| arg        | Object | No       | Subscribed channel                                                      |
| > channel  | String | Yes      | Channel name<br>`adl-warning`                                                            |
| > instType | String | Yes | Instrument type  |
| > instFamily | String | No | Instrument family |
| code | String | No | Error code |
| msg | String | No | Error message |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
   "arg":{
      "channel":"adl-warning",
      "instType":"FUTURES",
      "instFamily":"BTC-USDT"
   },
   "data":[
      {
         "instType":"FUTURES",
         "instFamily":"BTC-USDT",
         "state":"warning",
         "bal":"280784384.9564228289548144",
         "ccy":"",
         "maxBal":"",
         "maxBalTs":"",
         "adlType":"",
         "adlBal":"",
         "adlRecBal":"",
         "ts":"1700210763001",
         "decRate":"",
         "adlRate":"",
         "adlRecRate":""
      }
   ]
}

```

#### Push data parameters

| Parameter      | Type   | Description                                                                                                                                                                                                              |
|:---------------| :----- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg            | Object | Subscribed channel                                                                                                                                                                                                       |
| > channel      | String | Channel name<br>`adl-warning`                                                                                                                                                                                                             |
| > instType | String  | Instrument type |
| > instFamily | String  | Instrument family |
| data | Array of objects | Subscribed data |
| > instType | String | Instrument type  |
| > instFamily | String | Instrument family |
| > state | String | state <br>`warning` <br>`adl` |
| > bal | String | Real-time security fund balance  |
| > ccy | String | ~~The corresponding currency of security fund balance~~(Deprecated, returns `""`. To be removed in a future update) |
| > maxBal | String | ~~Maximum security fund balance in the past eight hours <br><br>Applicable when state is `warning` or `adl`~~(Deprecated, returns `""`. To be removed in a future update) |
| > maxBalTs | String | ~~Timestamp when security fund balance reached maximum in the past eight hours, Unix timestamp format in milliseconds, e.g. `1597026383085`~~(Deprecated, returns `""`. To be removed in a future update) |
| > adlType | String | ~~ADL related events <br>`rate_adl_start`: ADL begins due to high security fund decline rate <br>`bal_adl_start`: ADL begins due to security fund balance falling <br>`pos_adl_start`：ADL begins due to the volume of liquidation orders falls to a certain level (only applicable to premarket symbols) <br>`adl_end`: ADL ends~~(Deprecated, returns `""`. To be removed in a future update) |
| > adlBal | String | ~~security fund balance that triggers ADL~~(Deprecated, returns `""`. To be removed in a future update) |
| > adlRecBal | String | ~~security fund balance that turns off ADL~~(Deprecated, returns `""`. To be removed in a future update) |
| > ts | String | Data push time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| > decRate | String | ~~Real-time security fund decline rate (compare bal and maxBal) <br><br>Applicable when state is `warning` or `adl`~~(Deprecated) |
| > adlRate | String | ~~security fund decline rate that triggers ADL~~(Deprecated) |
| > adlRecRate | String | ~~security fund decline rate that turns off ADL~~(Deprecated) |
