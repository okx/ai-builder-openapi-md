---
site: okex
lang: en
name: Balance and position channel
type: guide
---

### Balance and position channel

Retrieve account balance and position information. Data will be pushed when triggered by events such as filled order, funding transfer.<br>
This channel applies to getting the account cash balance and the change of position asset ASAP. <br>
Concurrent connection to this channel will be restricted by the following rules: [WebSocket connection count limit] (/docs-v5/en/#overview-websocket-connection-count-limit).

#### URL Path

/ws/v5/private (required login)

> Request Example 

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "balance_and_position"
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
        url = "wss://ws.okx.com:8443/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "balance_and_position"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters 

| Parameter      | Type   | Required | Description                                                |
| :-------- | :----- | :------- | :-------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes       | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | Yes       | List of subscribed channels                                  |
| > channel | String | Yes       | Channel name<br>`balance_and_position`                                   |

> Response Example 

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "balance_and_position"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"balance_and_position\"}]}",
	"connId": "a4d3ae55"
}
```
#### Response parameters 

| Parameter       | Type   | Required | Description                                                         |
| :--------- | :----- | :------- | :----------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg        | Object | No       | List of subscribed channels                                                  |
| > channel  | String | Yes      | Channel name<br>`balance_and_position`                                            |
| code       | String | No       | Error code                                                       |
| msg        | String | No       | Error message                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
	"arg": {
		"channel": "balance_and_position",
		"uid": "77982378738415879"
	},
	"data": [{
		"pTime": "1597026383085",
		"eventType": "snapshot",
		"balData": [{
			"ccy": "BTC",
			"cashBal": "1",
			"uTime": "1597026383085"
		}],
		"posData": [{
			"posId": "1111111111",
			"tradeId": "2",
			"instId": "BTC-USD-191018",
			"instType": "FUTURES",
			"mgnMode": "cross",
			"posSide": "long",
			"pos": "10",
			"ccy": "BTC",
			"posCcy": "",
			"avgPx": "3320",
            "nonSettleAvgPx": "",
			"settledPnl": "",
			"uTime": "1597026383085"
		}],
		"trades": [{
			"instId": "BTC-USD-191018",
			"tradeId": "2",
		}]
	}]
}
```

#### Push data parameters

| **Parameter**     | **Type**         | **Description**                                                                                                                                                                                                            |
|:------------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg               | Object           | Channel to subscribe to                                                                                                                                                                                                    |
| > channel         | String           | Channel name                                                                                                                                                                                                               |
| > uid             | String           | User Identifier                                                                                                                                                                                                            |
| data              | Array of objects | Subscribed data                                                                                                                                                                                                            |
| > pTime           | String           | Push time of both balance and position information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                             |
| > eventType       | String           | Event Type<br>`snapshot`<br>`delivered`<br>`exercised`<br>`transferred`<br>`filled`<br>`liquidation`<br>`claw_back`<br>`adl`<br>`funding_fee`<br>`adjust_margin`<br>`set_leverage`<br>`interest_deduction`<br>`settlement` |
| > balData         | Array of objects | Balance data                                                                                                                                                                                                               |
| >> ccy            | String           | Currency                                                                                                                                                                                                                   |
| >> cashBal        | String           | Cash Balance                                                                                                                                                                                                               |
| >> uTime          | String           | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                   |
| > posData         | Array of objects | Position data                                                                                                                                                                                                              |
| >> posId          | String           | Position ID                                                                                                                                                                                                                |
| >> tradeId        | String           | Last trade ID                                                                                                                                                                                                              |
| >> instId         | String           | Instrument ID, e.g `BTC-USD-180213`                                                                                                                                                                                        |
| >> instType       | String           | Instrument type                                                                                                                                                                                                            |
| >> mgnMode        | String           | Margin mode<br>`isolated`, `cross`                                                                                                                                                                                         |
| >> avgPx          | String           | Average open price                                                                                                                                                                                                         |
| >> ccy            | String           | Currency used for margin                                                                                                                                                                                                   |
| >> posSide        | String           | Position side<br>`long`, `short`, `net`                                                                                                                                                                                    |
| >> pos            | String           | Quantity of positions. In the isolated margin mode, when doing manual transfers, a position with pos of `0` will be generated after the deposit is transferred                                                             |
| >> baseBal        | String           | ~~Base currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                      |
| >> quoteBal       | String           | ~~Quote currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                     |
| >> posCcy         | String           | Position currency, only applicable to MARGIN positions.                                                                                                                                                                    |
| >> nonSettleAvgPx | String    | Non-Settlement entry price<br>The non-settlement entry price only reflects the average price at which the position is opened or increased.<br>Applicable to `FUTURES` `cross`                           |
| >> settledPnl     | String    | Accumulated settled P&L (calculated by settlement price)<br>Applicable to `FUTURES` `cross` |
| >> uTime          | String           | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                   |
| > trades          | Array of objects  | Details of trade                                                                                                                                                                                                           |
| >> instId         | String           | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                             |
| >> tradeId        | String           | Trade ID                                                                                                                                                                                                                   |

<aside class="notice"> 
Only balData will be pushed if only the account balance changes; only posData will be pushed if only the position changes.
</aside> 

<aside class="notice"> 
<br> - Initial snapshot: Only either position with non-zero position quantity or cash balance with non-zero quantity will be pushed. If the data is too large to be sent in a single push message, it will be split into multiple messages.
<br> - For example, if you subscribe according to all currencies and the user has 5 currency balances that are not 0 and 20 positions, all 20 positions data and 5 currency balances data will be pushed in initial snapshot; Subsequently when there is change in pos of a position, only the data of that position will be pushed triggered by this change.
</aside>
