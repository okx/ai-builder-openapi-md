---
site: okex
lang: en
name: Account greeks channel
type: guide
---

### Account greeks channel

Retrieve account greeks information. Data will be pushed when triggered by events such as increase/decrease positions or cash balance in account, 
and will also be pushed in regular interval according to subscription granularity.<br>
Concurrent connection to this channel will be restricted by the following rules: [WebSocket connection count limit] (/docs-v5/en/#overview-websocket-connection-count-limit).

#### URL Path

/ws/v5/private (required login)

> Request Example

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "account-greeks"
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
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "account-greeks"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                 |
| :-------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes       | Operation<br>`subscribe`      `unsubscribe`                                                                                                                                                                                                                 |
| args      | Array of objects  | Yes       | List of subscribed channels                                                                                                                                                                                                                                 |
| > channel | String | Yes       | Channel name<br>`account-greeks`                                                                                                                                                                                                                            |
| > ccy     | String | No        | Settlement currency<br>When the user specifies a settlement currency, event push will only be triggered when the position of the same settlement currency changes. For example, when ccy=BTC, if the position of `BTC-USDT-SWAP` changes, no event push will be triggered. |

> Successful Response Example

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "account-greeks"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account-greeks\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter       | Type   | Required | Description                                                |
| :--------- | :----- | :------- | :-----------------------------------------------------------    |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error`            |
| arg        | Object | No       | Subscribed channel                                              |
| > channel  | String | Yes      | Channel name,`account-greeks`                                   |
| > ccy      | String | No       | Settlement currency                                                        |
| code       | String | No       | Error code                                                      |
| msg        | String | No       | Error message                                                   |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: single

```json
{
    "arg": {
        "channel": "account-greeks",
        "ccy": "BTC",
        "uid": "614488474791936"
    },
    "data": [
        {
            "ccy": "BTC",
            "deltaBS": "1.1246665401944310",
            "deltaPA": "-0.0074076183688949",
            "gammaBS": "0.0000000000000000",
            "gammaPA": "0.0148152367377899",
            "thetaBS": "2.0356991946421226",
            "thetaPA": "-0.0000000200174309",
            "ts": "1729179082006",
            "vegaBS": "0.0000000000000000",
            "vegaPA": "0.0000000000000000"
        }
    ]
}
```

> Push Data Example

```json
{
    "arg": {
        "channel": "account-greeks",
        "uid": "614488474791936"
    },
    "data": [
        {
            "ccy": "BTC",
            "deltaBS": "1.1246665403011684",
            "deltaPA": "-0.0074021163991037",
            "gammaBS": "0.0000000000000000",
            "gammaPA": "0.0148042327982075",
            "thetaBS": "2.1342098201092528",
            "thetaPA": "-0.0000000200876441",
            "ts": "1729179001692",
            "vegaBS": "0.0000000000000000",
            "vegaPA": "0.0000000000000000"
        },
        {
            "ccy": "ETH",
            "deltaBS": "0.3810670161698570",
            "deltaPA": "-0.0688347042402955",
            "gammaBS": "-0.0000000000230396",
            "gammaPA": "0.1376693483440320",
            "thetaBS": "0.3314776517141782",
            "thetaPA": "0.0000000001316008",
            "ts": "1729179001692",
            "vegaBS": "-0.0000000045069794",
            "vegaPA": "-0.0000000000017267"
        }
    ]
}
```
#### Push data parameters

| **Parameters** | **Types** | **Description**                                                   |
| :----------- | :------- | :----------------------------------------------------------- |
| arg           | Object   | Successfully subscribed channel                                                                                                        |
| > channel     | String   | Channel name                                                                                                                           |
| > uid         | String | User Identifier                                                                                                                          |
| data          | Array of objects    | Subscribed data                                                                                                                        |
| > deltaBS       | String   | delta: Black-Scholes Greeks in dollars     |
| > deltaPA       | String   | delta: Greeks in coins                     |
| > gammaBS       | String   | gamma: Black-Scholes Greeks in dollars, only applicable to OPTION cross|
| > gammaPA       | String   | gamma: Greeks in coins, only applicable to OPTION cross        |
| > thetaBS       | String   | theta: Black-Scholes Greeks in dollars, only applicable to OPTION cross          |
| > thetaPA       | String   | theta: Greeks in coins, only applicable to OPTION cross          |
| > vegaBS        | String   | vega: Black-Scholes Greeks in dollars, only applicable to OPTION cross           |
| > vegaPA        | String   | vega: Greeks in coins, only applicable to OPTION cross            |
| > ccy           | String   | Currency  |
| > ts            | String   | Push time of account greeks, Unix timestamp format in milliseconds, e.g. 1597026383085     |

<aside class="notice"> 
The account greeks data is sent on event basis and regular basis
<br> - The event push is not pushed in real-time. It is aggregated and pushed at a fixed time interval, around 50ms. For example, if multiple events occur within a fixed time interval, the system will aggregate them into a single message and push it at the end of the fixed time interval. If the data volume is too large, it may be split into multiple messages.
<br> - When the user specifies a settlement currency in the subscribe request, event push will only be triggered when the position of the same settlement currency changes. For example, when subscribe `ccy`=BTC, if the position of `BTC-USDT-SWAP` changes, no event push will be triggered.
<br> - The regular push sends updates regardless of whether there are activities or not.
</aside> 

<aside class="notice"> 
<br> - Only currencies in the account will be pushed. If the data is too large to be sent in a single push message, it will be split into multiple messages.
<br> - For example, when subscribing to account-greeks channel without specifying ccy and there are 5 currencies are with non-zero balance, all 5 currencies data will be pushed in initial snapshot and in regular interval. Subsequently when there is change in balance or equity of an token, only the incremental data of that currency will be pushed triggered by this change.
</aside>
