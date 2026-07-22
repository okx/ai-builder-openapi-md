---
site: okex
lang: en
name: WS / Recurring buy orders channel
type: guide
---

### WS / Recurring buy orders channel

Retrieve recurring buy orders. Data will be pushed when triggered by events. It will also be pushed in regular interval according to subscription granularity.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
	"id": "1512",
    "op": "subscribe",
	"args": [{
		"channel": "algo-recurring-buy",
		"instType": "SPOT"
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
		"channel": "algo-recurring-buy",
		"instType": "SPOT"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                       |
|:-----------| :----- | -------- | --------------------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                             |
| args       | Array of objects  | Yes      | List of subscribed channels                                                       |
| > channel  | String | Yes      | Channel name<br>`algo-recurring-buy`                                                                      |
| > instType | String | Yes      | Instrument type<br>`SPOT`<br>`ANY` |
| > algoId   | String | No       | Algo Order ID                                                       |

> Successful Response Example

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "algo-recurring-buy",
		"instType": "SPOT"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"algo-recurring-buy\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter  | Type   | Required | Description                                                                       |
|:-----------| :----- | -------- | --------------------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                         |
| arg        | Object | No       | Subscribed channel                                                                  |
| > channel  | String | Yes      | Channel name                                                                      |
| > instType | String | Yes      | Instrument type                                                                   |
| > algoId   | String | No       | Algo Order ID                                                                     |
| code       | String | No       | Error code                                                                        |
| msg        | String | No       | Error message                                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: 

```json
{
	"arg": {
		"channel": "algo-recurring-buy",
		"instType": "SPOT",
		"uid": "447*******584"
	},
	"data": [{
		"algoClOrdId": "",
		"algoId": "644497312047435776",
		"algoOrdType": "recurring",
		"amt": "100",
		"cTime": "1699932133373",
		"cycles": "0",
		"instType": "SPOT",
		"investmentAmt": "0",
		"investmentCcy": "USDC",
		"mktCap": "0",
		"nextInvestTime": "1699934415300",
		"pTime": "1699933314691",
		"period": "hourly",
		"pnlRatio": "0",
		"recurringDay": "",
		"recurringHour": "1",
		"recurringList": [{
			"avgPx": "0",
			"ccy": "BTC",
			"profit": "0",
			"px": "36482",
			"ratio": "0.2",
            "minPx": "30000",
            "maxPx": "50000"
			"totalAmt": "0"
		}, {
			"avgPx": "0",
			"ccy": "ETH",
			"profit": "0",
			"px": "2057.54",
			"ratio": "0.8",
            "minPx": "",
            "maxPx": "",
			"totalAmt": "0"
		}],
		"recurringTime": "12",
		"state": "running",
		"stgyName": "stg1",
		"tag": "",
		"timeZone": "8",
		"totalAnnRate": "0",
		"totalPnl": "0",
		"uTime": "1699932136249",
		"tradeQuoteCcy": "USDT"
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter**       | **Type**        | **Description**                                                                                                                                                                                                   |
|:--------------------|:----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                 | Object          | Successfully subscribed channel                                                                                                                                                                                   |
| > channel           | String          | Channel name                                                                                                                                                                                                      |
| > instType          | String          | Instrument type                                                                                                                                                                                                   |
| > algoId            | String          | Algo Order ID                                                                                                                                                                                                     |
| > uid               | String          | User ID                                                                                                                                                                                                           |
| data                | Array of objects           | Subscribed data                                                                                                                                                                                                   |
| > algoId            | String          | Algo ID                                                                                                                                                                                                           |
| > algoClOrdId       | String          | Client-supplied Algo ID                                                                                                                                                                                           |
| > instType          | String          | Instrument type                                                                                                                                                                                                   |
| > cTime             | String          | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                              |
| > uTime             | String          | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                              |
| > algoOrdType       | String          | Algo order type<br>`recurring`: recurring buy                                                                                                                                                                     |
| > state             | String          | Algo order state<br>`running`<br>`stopping`<br>`stopped`<br/>`pause`                                                                                                                                                          |
| > stgyName          | String          | Custom name for trading bot, no more than 40 characters                                                                                                                                                           |
| > recurringList     | Array of objects | Recurring buy info                                                                                                                                                                                                |
| >> ccy              | String          | Recurring buy currency, e.g. `BTC`                                                                                                                                                                                |
| >> ratio            | String          | Proportion of recurring currency assets, e.g. "0.2" representing 20%                                                                                                                                              |
| >> minPx | String | Minimum price of price range. `""` means no limit |
| >> maxPx | String | Maximum price of price range. `""` means no limit |
| >> totalAmt         | String          | Accumulated quantity in unit of recurring buy currency                                                                                                                                                            |
| >> profit           | String          | Profit in unit of `investmentCcy`                                                                                                                                                                                 |
| >> avgPx            | String          | Average price of recurring buy, quote currency is `investmentCcy`                                                                                                                                                 |
| >> px               | String          | Current market price, quote currency is `investmentCcy`                                                                                                                                                           |
| > period            | String          | Period<br>`monthly`<br>`weekly`<br>`daily`<br>`hourly`                                                                                                            |
| > recurringDay      | String          | Recurring buy date<br>When the period is `monthly`, the value range is an integer of [1,28]<br>When the period is `weekly`, the value range is an integer of [1,7] |
| > recurringHour     | String          | Recurring buy by hourly<br>`1`/`4`/`8`/`12`, e.g. `4` represents "recurring buy every 4 hour"                                                                     |
| > recurringTime     | String          | Recurring buy time, the value range is an integer of [0,23]                                                                                                                                                       |
| > timeZone          | String          | UTC time zone, the value range is an integer of [-12,14]<br>e.g. "8" representing UTC+8 (East 8 District), Beijing Time                                                                                            |
| > amt               | String          | Quantity invested per cycle                                                                                                                                                                                       |
| > investmentAmt     | String          | Accumulate quantity invested                                                                                                                                                                                      |
| > investmentCcy     | String          | The invested quantity unit, can only be `USDT`/`USDC`                                                                                                                                                             |
| > nextInvestTime        | String          | Next invest time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                |
| > totalPnl          | String          | Total P&L                                                                                                                                                                                                         |
| > totalAnnRate      | String          | Total annualized rate of yield                                                                                                                                                                                    |
| > pnlRatio          | String          | Rate of yield                                                                                                                                                                                                     |
| > mktCap            | String          | Market value in unit of `USDT`                                                                                                                                                                                    |
| > cycles            | String          | Accumulate recurring buy cycles                                                                                                                                                                                   |
| > tag               | String          | Order tag                                                                                                                                                                                                         |
| > pTime             | String          | Push time of algo order information, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                  |
| > tradeQuoteCcy     | String          | The quote currency for trading. |
| > recurringTimeType | String | Recurring buy time type |
| > recurringTimeMinutes | String | Custom recurring buy minutes |
| > source | Array | Source of recurring buy |
