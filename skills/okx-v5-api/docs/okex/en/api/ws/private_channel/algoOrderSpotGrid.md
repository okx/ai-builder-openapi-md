---
site: okex
lang: en
name: WS / Spot grid algo orders channel
type: guide
---

### WS / Spot grid algo orders channel

Retrieve spot grid algo orders. Data will be pushed when triggered by events such as placing/canceling order. It will also be pushed in regular interval according to subscription granularity.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-orders-spot",
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
		"channel": "grid-orders-spot",
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
| :--------- | :----- | -------- | --------------------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                             |
| args     | Array of objects  | Yes      | List of subscribed channels                                                       |
| > channel  | String | Yes      | Channel name<br>`grid-orders-spot`                                                                      |
| > instType | String | Yes      | Instrument type<br>`SPOT`<br>`ANY` |
| > instId   | String | No       | Instrument ID                                                                     |
| > algoId   | String | No       | Algo Order ID                                                       |

> Successful Response Example

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-orders-spot",
		"instType": "ANY"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-orders-spot\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter  | Type   | Required | Description                                                                       |
| :--------- | :----- | -------- | --------------------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                         |
| arg      | Object | No       | Subscribed channel                                                                  |
| > channel  | String | Yes      | Channel name                                                                      |
| > instType | String | Yes      | Instrument type                                                                   |
| > instId   | String | No       | Instrument ID                                                                     |
| > algoId   | String | No       | Algo Order ID                                                                     |
| code       | String | No       | Error code                                                                        |
| msg        | String | No       | Error message                                                                     |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example: 

```json
{
	"arg": {
		"channel": "grid-orders-spot",
		"instType": "ANY",
		"uid": "44705892343619584"
	},
	"data": [{
        "algoClOrdId": "",
		"algoId": "568028283477164032",
        "activeOrdNum" : "10",
		"algoOrdType": "grid",
		"annualizedRate": "0",
		"arbitrageNum": "0",
		"baseSz": "0",
		"cTime": "1681700496249",
		"cancelType": "0",
		"curBaseSz": "0",
		"curQuoteSz": "25",
		"floatProfit": "0",
		"gridNum": "10",
		"gridProfit": "0",
		"instId": "BTC-USDT",
		"instType": "SPOT",
		"investment": "25",
		"maxPx": "5000",
		"minPx": "400",
		"pTime": "1682416738467",
		"perMaxProfitRate": "1.14570215",
		"perMinProfitRate": "0.0991200440528634356837",
		"pnlRatio": "0",
		"profit": "0",
		"quoteSz": "25",
		"rebateTrans": [{
			"rebate": "0",
			"rebateCcy": "BTC"
		}, {
			"rebate": "0",
			"rebateCcy": "USDT"
		}],
		"runPx": "30031.7",
		"runType": "1",
		"triggerParams": [{
			"triggerAction": "start",
			"triggerStrategy": "instant",
			"delaySeconds": "0",
			"triggerType": "auto",
			"triggerTime": ""
		}, {
			"triggerAction": "stop",
			"triggerStrategy": "instant",
			"delaySeconds": "0",
			"stopType": "1",
			"triggerType": "manual",
			"triggerTime": ""
		}],
		"singleAmt": "0.00101214",
		"slTriggerPx": "",
		"state": "running",
		"stopResult": "0",
		"stopType": "2",
		"tag": "",
		"totalAnnualizedRate": "0",
		"totalPnl": "0",
		"tpTriggerPx": "",
		"tradeNum": "0",
		"uTime": "1682406665527",
        "profitSharingRatio": "", 
        "copyType": "0",
		"tradeQuoteCcy": "USDT"
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter**      | **Type** | **Description**                                                                                                                                                                                    |
|:-------------------| :------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                | Object   | Successfully subscribed channel                                                                                                                                                                    |
| > channel          | String   | Channel name                                                                                                                                                                                       |
| > instType         | String   | Instrument type                                                                                                                                                                                    |
| > uid              | String   | User ID                                                                                                                                                                                            |
| data               | Array of objects    | Subscribed data                                                                                                                                                                                    |
| > algoId           | String   | Algo ID                                                                                                                                                                                            |
| > algoClOrdId         | String   | Client-supplied Algo ID                                                    |
| > instType         | String   | Instrument type                                                                                                                                                                                    |
| > instId           | String   | Instrument ID                                                                                                                                                                                      |
| > cTime            | String   | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                               |
| > uTime            | String   | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                               |
| > algoOrdType      | String   | Algo order type<br>`grid`: Spot grid                                                                                                                                                               |
| > state            | String   | Algo order state<br>`starting`<br>`running`<br>`stopping`<br>`stopped`                                                                                                                             |
| > rebateTrans      | Array of objects   | Rebate transfer info                                                                                                                                                                               |
| >> rebate          | String   | Rebate amount                                                                                                                                                                                      |
| >> rebateCcy       | String   | Rebate currency                                                                                                                                                                                    |
| > triggerParams    | Array of objects   | Trigger Parameters                                                                                                                                                                                 |
| >> triggerAction   | String   | Trigger action<br>`start`<br>`stop`                                                                                                                                                                |
| >> triggerStrategy | String   | Trigger strategy<br>`instant`<br>`price`<br>`rsi`                                                                                                                                                  |
| >> delaySeconds    | String   | Delay seconds after action triggered                                                                                                                                                               |
| >> triggerTime     | String   | Actual action triggered time, unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                          |
| >> triggerType     | String   | Actual action triggered type<br>`manual`<br>`auto`                                                                                                                                                 |
| >> timeframe       | String   | K-line type<br>`3m`, `5m`, `15m`, `30m` (`m`: minute)<br>`1H`, `4H` (`H`: hour)<br>`1D` (`D`: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                     |
| >> thold           | String   | Threshold<br>The value should be an integer between 1 to 100<br>This field is only valid when `triggerStrategy` is `rsi`                                                                           |
| >> triggerCond     | String   | Trigger condition<br>`cross_up`<br>`cross_down`<br>`above`<br>`below`<br>`cross`<br>This field is only valid when `triggerStrategy` is `rsi`                                                       |
| >> timePeriod      | String   | Time Period<br>`14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                    |
| >> triggerPx       | String   | Trigger Price<br>This field is only valid when `triggerStrategy` is `price`                                                                                                                        |
| >> stopType        | String   | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions<br>This field is only valid when `triggerAction` is `stop` |
| > maxPx               | String   | Upper price of price range                                                                                                                                                                         |
| > minPx               | String   | Lower price of price range                                                                                                                                                                         |
| > gridNum             | String   | Grid quantity                                                                                                                                                                                      |
| > runType             | String   | Grid type<br>`1`: Arithmetic, `2`: Geometric                                                                                                                                                       |
| > tpTriggerPx         | String   | Take-profit trigger price                                                                                                                                                                          |
| > slTriggerPx         | String   | Stop-loss trigger price                                                                                                                                                                            |
| > tradeNum            | String   | The number of trades executed                                                                                                                                                                      |
| > arbitrageNum        | String   | The number of arbitrages executed                                                                                                                                                                  |
| > singleAmt           | String   | Amount per grid	                                                                                                                                                                                   |
| > perMinProfitRate    | String   | Estimated minimum Profit margin per grid                                                                                                                                                           |
| > perMaxProfitRate    | String   | Estimated maximum Profit margin per grid                                                                                                                                                           |
| > runPx               | String   | Price at launch                                                                                                                                                                                    |
| > totalPnl            | String   | Total P&L                                                                                                                                                                                          |
| > pnlRatio            | String   | P&L ratio                                                                                                                                                                                          |
| > investment          | String   | Investment amount<br>Spot grid investment amount calculated on quote currency                                                                                                                      |
| > gridProfit          | String   | Grid profit                                                                                                                                                                                        |
| > floatProfit         | String   | Variable P&L                                                                                                                                                                                       |
| > totalAnnualizedRate | String   | Total annualized rate                                                                                                                                                                              |
| > annualizedRate      | String   | Grid annualized rate                                                                                                                                                                               |
| > cancelType          | String   | Algo order stop reason<br>`0`: None<br>`1`: Manual stop<br>`2`: Take profit<br>`3`: Stop loss<br>`4`: Risk control<br>`5`: Delivery<br>`6`: Signal                                                 |
| > stopType            | String   | Stop type<br>`1`: Sell base currency `2`: Keep base currency                                                                                                                                       |
| > quoteSz             | String   | Quote currency investment amount<br>Only applicable to `Spot grid`                                                                                                                                 |
| > baseSz              | String   | Base currency investment amount<br>Only applicable to `Spot grid`                                                                                                                                  |
| > curQuoteSz          | String   | Assets of quote currency currently held<br>Only applicable to `Spot grid`                                                                                                                          |
| > curBaseSz           | String   | Assets of base currency currently held<br>Only applicable to `Spot grid`                                                                                                                           |
| > profit              | String   | Current available profit based on quote currency<br>Only applicable to `Spot grid`                                                                                                                 |
| > stopResult          | String   | Stop result<br>`0`: default, `1`: Successful selling of currency at market price, `-1`: Failed to sell currency at market price<br>Only applicable to `Spot grid`                                  |
| > activeOrdNum        | String   | Total count of pending sub orders                                                                                                                                                                  |
| > tag                 | String   | Order tag                                                                                                                                                                                          |
| > profitSharingRatio  | String   | Profit sharing ratio<br>Value range [0, 0.3]<br>If it is a normal order (neither copy order nor lead order), this field returns ""                                                                |
| > copyType            | String   | Profit sharing order type<br>`0`: Normal order<br>`1`: Copy order without profit sharing<br>`2`: Copy order with profit sharing<br>`3`: Lead order                            |
| > pTime               | String   | Push time of algo grid information, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                    |
| > tradeQuoteCcy       | String   | The quote currency for trading. |
