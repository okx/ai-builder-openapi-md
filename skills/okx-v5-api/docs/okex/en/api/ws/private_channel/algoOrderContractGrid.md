---
site: okex
lang: en
name: WS / Contract grid algo orders channel
type: guide
---

### WS / Contract grid algo orders channel

Retrieve contract grid algo orders. Data will be pushed when triggered by events such as placing/canceling order. It will also be pushed in regular interval according to subscription granularity.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
  "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-orders-contract",
		"instType": "SWAP"
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "grid-orders-contract",
		"instType": "SWAP"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                     |
| :--------- | :----- | -------- | ------------------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                           |
| args     | Array of objects  | Yes      | List of subscribed channels                                                     |
| > channel  | String | Yes      | Channel name<br>`grid-orders-contract`                                                                    |
| > instType | String | Yes      | Instrument type<br>`SWAP`<br>`FUTURES`<br>`ANY` |
| > instId   | String | No       | Instrument ID                                                                   |
| > algoId   | String | No       | Algo Order ID                                                     |

> Successful Response Example

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-orders-contract",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-orders-contract\", \"instType\" : \"FUTURES\"}]}",
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
		"channel": "grid-orders-contract",
		"instType": "ANY",
		"uid": "4470****9584"
	},
	"data": [{
		"actualLever": "2.3481494635276649",
        "activeOrdNum": "10",
        "algoClOrdId": "",
		"algoId": "571039869070475264",
		"algoOrdType": "contract_grid",
		"annualizedRate": "0",
		"arbitrageNum": "0",
		"availEq": "52.3015392887089673",
		"basePos": true,
		"cTime": "1682418514204",
		"cancelType": "0",
		"direction": "long",
		"eq": "108.7945652387089673",
		"floatProfit": "8.7945652387089673",
		"gridNum": "10",
		"gridProfit": "0",
		"instId": "BTC-USDT-SWAP",
		"instType": "SWAP",
		"investment": "100",
		"lever": "5",
		"liqPx": "16370.482143120824",
		"maxPx": "36437.3",
		"minPx": "26931.9",
		"ordFrozen": "5.38638",
		"pTime": "1682492574068",
		"perMaxProfitRate": "0.1687494513302446",
		"perMinProfitRate": "0.1263869357706788",
		"pnlRatio": "0.0879456523870897",
		"rebateTrans": [{
			"rebate": "0",
			"rebateCcy": "USDT"
		}],
		"runPx": "27306.9",
		"runType": "1",
		"singleAmt": "1",
		"slTriggerPx": "",
		"state": "running",
		"stopType": "0",
		"sz": "100",
		"tag": "",
		"totalAnnualizedRate": "38.52019574554529",
		"totalPnl": "8.7945652387089673",
		"tpTriggerPx": "",
		"tradeNum": "9",
		"triggerParams": [{
			"triggerAction": "start",
			"delaySeconds": "0",
			"triggerStrategy": "price",
			"triggerPx": "1",
			"triggerType": "manual",
			"triggerTime": "1682418561497"
		}, {
			"triggerAction": "stop",
			"delaySeconds": "0",
			"triggerStrategy": "instant",
			"stopType": "1",
			"triggerType": "manual",
			"triggerTime": "0"
		}],
		"uTime": "1682492552257",
        "profitSharingRatio": "",
        "copyType": "0",
        "tpRatio": "",
        "slRatio": "",
        "fee": "",
        "fundingFee": ""
	}]
}
```

#### Response parameters when data is pushed.

| **Parameter**         | **Type** | **Description**                                                                                                                                                                                        |
|:----------------------| :------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                   | Object   | Successfully subscribed channel                                                                                                                                                                        |
| > channel             | String   | Channel name                                                                                                                                                                                           |
| > instType            | String   | Instrument type                                                                                                                                                                                        |
| > uid                 | String   | User ID                                                                                                                                                                                                |
| data                  | Array of objects    | Subscribed data                                                                                                                                                                                        |
| > algoId              | String   | Algo ID                                                                                                                                                                                                |
| > algoClOrdId         | String   | Client-supplied Algo ID                                                                                                                                                                                |
| > instType            | String   | Instrument type                                                                                                                                                                                        |
| > instId              | String   | Instrument ID                                                                                                                                                                                          |
| > cTime               | String   | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |
| > uTime               | String   | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |
| > algoOrdType         | String   | Algo order type<br>`contract_grid`: Contract grid                                                                                                                                                      |
| > state               | String   | Algo order state<br>`starting`<br>`running`<br>`stopping`<br>`no_close_position`: stopped algo order but hadn't close position yet<br>`stopped`                                                        |
| > rebateTrans         | Array of objects   | Rebate transfer info                                                                                                                                                                                   |
| >> rebate             | String   | Rebate amount                                                                                                                                                                                          |
| >> rebateCcy          | String   | Rebate currency                                                                                                                                                                                        |
| > triggerParams       | Array of objects   | Trigger Parameters                                                                                                                                                                                     |
| >> triggerAction      | String   | Trigger action<br>`start`<br>`stop`                                                                                                                                                                    |
| >> triggerStrategy    | String   | Trigger strategy<br>`instant`<br>`price`<br>`rsi`                                                                                                                                                      |
| >> delaySeconds       | String   | Delay seconds after action triggered                                                                                                                                                                   |
| >> triggerTime        | String   | Actual action triggered time, unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                              |
| >> triggerType        | String   | Actual action triggered type<br>`manual`<br>`auto`                                                                                                                                                     |
| >> timeframe          | String   | K-line type<br>`3m`, `5m`, `15m`, `30m` (`m`: minute)<br>`1H`, `4H` (`H`: hour)<br>`1D` (`D`: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                         |
| >> thold              | String   | Threshold<br>The value should be an integer between 1 to 100<br>This field is only valid when `triggerStrategy` is `rsi`                                                                               |
| >> triggerCond        | String   | Trigger condition<br>`cross_up`<br>`cross_down`<br>`above`<br>`below`<br>`cross`<br>This field is only valid when `triggerStrategy` is `rsi`                                                           |
| >> timePeriod         | String   | Time Period<br>`14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                        |
| >> triggerPx          | String   | Trigger Price<br>This field is only valid when `triggerStrategy` is `price`                                                                                                                            |
| >> stopType           | String   | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions<br>This field is only valid when `triggerAction` is `stop` |
| > maxPx               | String   | Upper price of price range                                                                                                                                                                             |
| > minPx               | String   | Lower price of price range                                                                                                                                                                             |
| > gridNum             | String   | Grid quantity                                                                                                                                                                                          |
| > runType             | String   | Grid type<br>`1`: Arithmetic, `2`: Geometric                                                                                                                                                           |
| > tpTriggerPx         | String   | Take-profit trigger price                                                                                                                                                                              |
| > slTriggerPx         | String   | Stop-loss trigger price                                                                                                                                                                                |
| > tradeNum            | String   | The number of trades executed                                                                                                                                                                          |
| > arbitrageNum        | String   | The number of arbitrages executed                                                                                                                                                                      |
| > singleAmt           | String   | Amount per grid	                                                                                                                                                                                       |
| > perMinProfitRate    | String   | Estimated minimum Profit margin per grid                                                                                                                                                               |
| > perMaxProfitRate    | String   | Estimated maximum Profit margin per grid                                                                                                                                                               |
| > runPx               | String   | Price at launch                                                                                                                                                                                        |
| > totalPnl            | String   | Total P&L                                                                                                                                                                                              |
| > pnlRatio            | String   | P&L ratio                                                                                                                                                                                              |
| > investment          | String   | Accumulated investment amount<br>Spot grid investment amount calculated on quote currency                                                                                                              |
| > gridProfit          | String   | Grid profit                                                                                                                                                                                            |
| > floatProfit         | String   | Variable P&L                                                                                                                                                                                           |
| > totalAnnualizedRate | String   | Total annualized rate                                                                                                                                                                                  |
| > annualizedRate      | String   | Grid annualized rate                                                                                                                                                                                   |
| > cancelType          | String   | Algo order stop reason<br>`0`: None<br>`1`: Manual stop<br>`2`: Take profit<br>`3`: Stop loss<br>`4`: Risk control<br>`5`: Delivery<br>`6`: Signal                                                     |
| > stopType            | String   | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions                                                            |
| > direction           | String   | Contract grid type<br>`long`,`short`,`neutral`<br>Only applicable to `contract grid`                                                                                                                   |
| > basePos             | Boolean   | Whether or not to open a position when the strategy is activated<br>Only applicable to `contract grid`                                                                                                 |
| > sz                  | String   | Used margin based on `USDT`<br>Only applicable to `contract grid`                                                                                                                                      |
| > lever               | String   | Leverage<br>Only applicable to `contract grid`                                                                                                                                                         |
| > actualLever         | String   | Actual Leverage<br>Only applicable to `contract grid`                                                                                                                                                  |
| > liqPx               | String   | Estimated liquidation price<br>Only applicable to `contract grid`                                                                                                                                      |
| > ordFrozen           | String   | Margin used by pending orders<br>Only applicable to `contract grid`                                                                                                                                    |
| > availEq             | String   | Available margin<br>Only applicable to `contract grid`                                                                                                                                                 |
| > eq             | String   | Total equity of strategy account<br>Only applicable to `contract grid`                                                                                                                                                 |
| > activeOrdNum        | String   | Total count of pending sub orders                                                                                                                                                                      |
| > tag                 | String   | Order tag                                                                                                                                                                                              |
| > profitSharingRatio  | String   | Profit sharing ratio<br>Value range [0, 0.3]<br>If it is a normal order (neither copy order nor lead order), this field returns ""                                                                     |
| > copyType            | String   | Profit sharing order type<br>`0`: Normal order<br>`1`: Copy order without profit sharing<br>`2`: Copy order with profit sharing<br>`3`: Lead order                                                     |
| > tpRatio             | String          | Take profit ratio, 0.1 represents 10%                                                                                                                                                                  |
| > slRatio             | String   | Stop loss ratio, 0.1 represents 10%                                                                                                                                                                    |
| > fee                 | String          | Accumulated fee. Only applicable to contract grid, or it will be ""                                                                                                                                    |
| > fundingFee          | String   | Accumulated funding fee. Only applicable to contract grid, or it will be ""                                                                                                                            |
| > pTime               | String   | Push time of algo grid information, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                        |
