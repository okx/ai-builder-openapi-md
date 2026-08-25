---
site: okex
lang: en
name: Positions channel
type: guide
---

### Positions channel

Retrieve position information. Initial snapshot will be pushed according to subscription granularity. Data will be pushed when triggered by events such as placing/canceling order, and will also be pushed in regular interval according to subscription granularity.<br>

Concurrent connection to this channel will be restricted by the following rules: [WebSocket connection count limit] (/docs-v5/en/#overview-websocket-connection-count-limit).

#### URL Path

/ws/v5/private (required login)

> Request Example : single

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "positions",
      "instType": "FUTURES",
      "instFamily": "BTC-USD"
    }
  ]
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
    args = [
        {
          "channel": "positions",
          "instType": "FUTURES",
          "instFamily": "BTC-USD"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "positions",
      "instType": "ANY",
      "extraParams": "
        {
          \"updateInterval\": \"0\"
        }
      "
    }
  ]
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
    args = [
        {
            "channel": "positions",
            "instType": "ANY",
            "extraParams": "{\"updateInterval\": \"0\"}"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters

| Parameter         | Type             | Required | Description                                                                        |
|:------------------|:-----------------|----------|------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op                | String           | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                                              |
| args              | Array of objects | Yes      | List of subscribed channels                                                        |
| > channel         | String           | Yes      | Channel name<br>`positions`                                                          |
| > instType        | String           | Yes      | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`  <br>`EVENTS`<br>`ANY` |
| > instFamily      | String           | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                              |
| > instId          | String           | No       | Instrument ID<br>If instId and instFamily are both passed, instId will be used                                                                      |
| > extraParams     | String           | No       | Additional configuration |
| >> updateInterval | int              | No       | `0`: only push due to positions events <br>`2000, 3000, 4000`: push by events and regularly according to the time interval setting (ms) <br><br>The data will be pushed both by events and around per 5 seconds regularly if this field is omitted or set to other values than the valid values above. <br><br>The following format should be strictly followed when using this field. <br>"extraParams": " <br>{ <br>\"updateInterval\": \"0\" <br>}<br>" |

> Successful Response Example : single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "positions",
    "instType": "FUTURES",
    "instFamily": "BTC-USD"
  },
  "connId": "a4d3ae55"
}
```

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "positions",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"positions\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter    | Type     | Required | Description                                                             |
|:-------------|:---------|----------|-------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event        | String   | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                        |
| arg          | Object   | No       | Subscribed channel                                                      |
| > channel    | String   | Yes      | Channel name                                                            |
| > instType   | String   | Yes      | Instrument type<br>`MARGIN`<br>`FUTURES`<br>`SWAP`<br>`OPTION`<br>`EVENTS`<br>`ANY` |
| > instFamily | String   | No       | Instrument family                                                       |
| > instId     | String   | No       | Instrument ID                                                           |
| code         | String   | No       | Error code                                                              |
| msg          | String   | No       | Error message                                                           |
| connId       | String   | Yes      | WebSocket connection ID                                                 |

> Push Data Example: single

```json
{
  "arg":{
      "channel":"positions",
      "uid": "77982378738415879",
      "instType":"FUTURES"
  },
  "eventType": "snapshot",
  "curPage": 1,
  "lastPage": true,
  "data":[
    {
      "adl":"1",
      "availPos":"1",
      "avgPx":"2566.31",
      "cTime":"1619507758793",
      "ccy":"ETH",
      "deltaBS":"",
      "deltaPA":"",
      "gammaBS":"",
      "gammaPA":"",
      "hedgedPos":"",
      "imr":"",
      "instId":"ETH-USD-210430",
      "instType":"FUTURES",
      "interest":"0",
      "idxPx":"2566.13",
      "last":"2566.22",
      "lever":"10",
      "liab":"",
      "liabCcy":"",
      "liqPx":"2352.8496681818233",
      "markPx":"2353.849",
      "margin":"0.0003896645377994",
      "mgnMode":"isolated",
      "mgnRatio":"11.731726509588816",
      "mmr":"0.0000311811092368",
      "notionalUsd":"2276.2546609009605",
      "optVal":"",
      "pTime":"1619507761462",
      "pendingCloseOrdLiabVal":"0.1",
      "pos":"1",
      "baseBorrowed": "",
      "baseInterest": "",
      "quoteBorrowed": "",
      "quoteInterest": "",
      "posCcy":"",
      "posId":"307173036051017730",
      "posSide":"long",
      "spotInUseAmt": "",
      "clSpotInUseAmt": "",
      "maxSpotInUseAmt": "",
      "bizRefId": "",
      "bizRefType": "",
      "spotInUseCcy": "",
      "thetaBS":"",
      "thetaPA":"",
      "tradeId":"109844",
      "uTime":"1619507761462",
      "upl":"-0.0000009932766034",
      "uplLastPx":"-0.0000009932766034",
      "uplRatio":"-0.0025490556801078",
      "uplRatioLastPx":"-0.0025490556801078",
      "vegaBS":"",
      "vegaPA":"",
      "realizedPnl":"0.001",
      "pnl":"0.0011",
      "fee":"-0.0001",
      "fundingFee":"0",
      "liqPenalty":"0",
      "nonSettleAvgPx":"", 
      "settledPnl":"",
      "closeOrderAlgo":[
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.6"
          },
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.4"
          }
      ]
    }
  ]
}
```

> Push Data Example

```json
{
  "arg":{
      "channel":"positions",
      "uid": "77982378738415879",
      "instType":"ANY"
  },
  "eventType": "snapshot",
  "curPage": 1,
  "lastPage": true,
  "data":[
    {
      "adl":"1",
      "availPos":"1",
      "avgPx":"2566.31",
      "cTime":"1619507758793",
      "ccy":"ETH",
      "deltaBS":"",
      "deltaPA":"",
      "gammaBS":"",
      "gammaPA":"",
      "hedgedPos":"",
      "imr":"",
      "instId":"ETH-USD-210430",
      "instType":"FUTURES",
      "interest":"0",
      "idxPx":"2566.13",
      "last":"2566.22",
      "usdPx":"",
      "bePx":"2353.949",
      "lever":"10",
      "liab":"",
      "liabCcy":"",
      "liqPx":"2352.8496681818233",
      "markPx":"2353.849",
      "margin":"0.0003896645377994",
      "mgnMode":"isolated",
      "mgnRatio":"11.731726509588816",
      "mmr":"0.0000311811092368",
      "notionalUsd":"2276.2546609009605",
      "optVal":"",
      "pTime":"1619507761462",
      "pendingCloseOrdLiabVal":"0.1",
      "pos":"1",
      "baseBorrowed": "",
      "baseInterest": "",
      "quoteBorrowed": "",
      "quoteInterest": "",
      "posCcy":"",
      "posId":"307173036051017730",
      "posSide":"long",
      "spotInUseAmt": "",
      "clSpotInUseAmt": "",
      "maxSpotInUseAmt": "",
      "spotInUseCcy": "",
      "bizRefId": "",
      "bizRefType": "",
      "thetaBS":"",
      "thetaPA":"",
      "tradeId":"109844",
      "uTime":"1619507761462",
      "upl":"-0.0000009932766034",
      "uplLastPx":"-0.0000009932766034",
      "uplRatio":"-0.0025490556801078",
      "uplRatioLastPx":"-0.0025490556801078",
      "vegaBS":"",
      "vegaPA":"",
      "realizedPnl":"0.001",
      "pnl":"0.0011",
      "fee":"-0.0001",
      "fundingFee":"0",
      "liqPenalty":"0",
      "nonSettleAvgPx":"", 
      "settledPnl":"",
      "closeOrderAlgo":[
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.6"
          },
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.4"
          }
      ]
    }, {
      "adl":"1",
      "availPos":"1",
      "avgPx":"2566.31",
      "cTime":"1619507758793",
      "ccy":"ETH",
      "deltaBS":"",
      "deltaPA":"",
      "gammaBS":"",
      "gammaPA":"",
      "hedgedPos":"",
      "imr":"",
      "instId":"ETH-USD-SWAP",
      "instType":"SWAP",
      "interest":"0",
      "idxPx":"2566.13",
      "last":"2566.22",
      "usdPx":"",
      "bePx":"2353.949",
      "lever":"10",
      "liab":"",
      "liabCcy":"",
      "liqPx":"2352.8496681818233",
      "markPx":"2353.849",
      "margin":"0.0003896645377994",
      "mgnMode":"isolated",
      "mgnRatio":"11.731726509588816",
      "mmr":"0.0000311811092368",
      "notionalUsd":"2276.2546609009605",
      "optVal":"",
      "pTime":"1619507761462",
      "pendingCloseOrdLiabVal":"0.1",
      "pos":"1",
      "baseBorrowed": "",
      "baseInterest": "",
      "quoteBorrowed": "",
      "quoteInterest": "",
      "posCcy":"",
      "posId":"307173036051017730",
      "posSide":"long",
      "spotInUseAmt": "",
      "clSpotInUseAmt": "",
      "maxSpotInUseAmt": "",
      "spotInUseCcy": "",
      "bizRefId": "",
      "bizRefType": "",
      "thetaBS":"",
      "thetaPA":"",
      "tradeId":"109844",
      "uTime":"1619507761462",
      "upl":"-0.0000009932766034",
      "uplLastPx":"-0.0000009932766034",
      "uplRatio":"-0.0025490556801078",
      "uplRatioLastPx":"-0.0025490556801078",
      "vegaBS":"",
      "vegaPA":"",
      "realizedPnl":"0.001",
      "pnl":"0.0011",
      "fee":"-0.0001",
      "fundingFee":"0",
      "liqPenalty":"0",
      "nonSettleAvgPx":"", 
      "settledPnl":"",
      "closeOrderAlgo":[
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.6"
          },
          {
              "algoId":"123",
              "slTriggerPx":"123",
              "slTriggerPxType":"mark",
              "tpTriggerPx":"123",
              "tpTriggerPxType":"mark",
              "closeFraction":"0.4"
          }
      ]
    }
  ]
}
```

#### Push data parameters

| Parameter                | Type   | Description                                                                                                                                                                                                                                                                                                                                                          |
|:-------------------------| :----- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                      | Object | Successfully subscribed channel                                                                                                                                                                                                                                                                                                                                      |
| > channel                | String | Channel name                                                                                                                                                                                                                                                                                                                                                         |
| > uid                    | String | User Identifier                                                                                                                                                                                                                                                                                                                                                      |
| > instType               | String | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| > instFamily             | String | Instrument family                                                                                                                                                                                                                                                                                                                                                    |
| > instId                 | String | Instrument ID                                                                                                                                                                                                                                                                                                                                                        |
| eventType                | String | Event type: <br>`snapshot`: Initial and regular snapshot push <br>`event_update`: Event-driven update push                                                                                                                                                                                                                                                           |
| curPage                  | Integer| Current page number. <br>Only applicable for `snapshot` events. Not included in `event_update` events.                                                                                                                                                                                                                                                               |
| lastPage                 | Boolean| Whether this is the last page of pagination:<br>`true`<br>`false`<br>Only applicable for `snapshot` events. Not included in `event_update` events.                                                                                                                                                                                                                   |
| data                     | Array of objects  | Subscribed data                                                                                                                                                                                                                                                                                                                                                      |
| > instType               | String | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| > mgnMode                | String | Margin mode, `cross` `isolated`                                                                                                                                                                                                                                                                                                                                      |
| > posId                  | String | Position ID                                                                                                                                                                                                                                                                                                                                                          |
| > posSide                | String | Position side<br>`long` <br>`short` <br>`net` (`FUTURES`/`SWAP`/`OPTION`: positive `pos` means long position and negative `pos` means short position. `MARGIN`: `posCcy` being base currency means long position, `posCcy` being quote currency means short position.)                                                                                               |
| > pos                    | String | Quantity of positions. In the isolated margin mode, when doing manual transfers, a position with pos of `0` will be generated after the deposit is transferred                                                                                                                                                                                                       |
| > hedgedPos | String | Hedged position size<br>Only return for accounts in delta neutral strategy, stgyType:1. Return "" for accounts in general strategy. |
| > baseBal                | String | ~~Base currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                                |
| > quoteBal               | String | ~~Quote currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                               |
| > baseBorrowed           | String | ~~Base currency amount already borrowed, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                  |
| > baseInterest           | String | ~~Base Interest, undeducted interest that has been incurred, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                              |
| > quoteBorrowed          | String | ~~Quote currency amount already borrowed, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                 |
| > quoteInterest          | String | ~~Quote Interest, undeducted interest that has been incurred, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                             |
| > posCcy                 | String | Position currency, only applicable to `MARGIN` positions                                                                                                                                                                                                                                                                                                             |
| > availPos               | String | Position that can be closed <br>Only applicable to `MARGIN` and `OPTION`. <br>For `Margin` position, the rest of sz will be `SPOT` trading after the liability is repaid while closing the position. Please get the available reduce-only amount from "Get maximum available tradable amount" if you want to reduce the amount of `SPOT` trading as much as possible. |
| > avgPx                  | String | Average open price                                                                                                                                                                                                                                                                                                                                                   |
| > upl                    | String | Unrealized profit and loss calculated by mark price.                                                                                                                                                                                                                                                                                                                 |
| > uplRatio               | String | Unrealized profit and loss ratio calculated by mark price.                                                                                                                                                                                                                                                                                                           |
| > uplLastPx              | String | Unrealized profit and loss calculated by last price. Main usage is showing, actual value is upl.                                                                                                                                                                                                                                                                     |
| > uplRatioLastPx         | String | Unrealized profit and loss ratio calculated by last price. 	                                                                                                                                                                                                                                                                                                         |
| > instId                 | String | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                                                                                                                  |
| > lever                  | String | Leverage, not applicable to `OPTION` seller                                                                                                                                                                                                                                                                                                                          |
| > liqPx                  | String | Estimated liquidation price <br>Not applicable to `OPTION`                                                                                                                                                                                                                                                                                                           |
| > markPx                 | String | Latest Mark price                                                                                                                                                                                                                                                                                                                                                    |
| > imr                    | String | Initial margin requirement, only applicable to `cross`                                                                                                                                                                                                                                                                                                               |
| > margin                 | String | Margin, can be added or reduced. Only applicable to `isolated` `Margin`.                                                                                                                                                                                                                                                                                             |
| > mgnRatio               | String | Maintenance margin ratio                                                                                                                                                                                                                                                                                                                                                         |
| > mmr                    | String | Maintenance margin requirement                                                                                                                                                                                                                                                                                                                                       |
| > liab                   | String | Liabilities, only applicable to `MARGIN`.                                                                                                                                                                                                                                                                                                                            |
| > liabCcy                | String | Liabilities currency, only applicable to `MARGIN`.                                                                                                                                                                                                                                                                                                                   |
| > interest               | String | Interest accrued that has not been settled.                                                                                                                                                                                                                                                                                                                          |
| > tradeId                | String | Last trade ID                                                                                                                                                                                                                                                                                                                                                        |
| > notionalUsd            | String | Notional value of positions in `USD`                                                                                                                                                                                                                                                                                                                                 |
| > optVal                 | String | Option Value, only applicable to `OPTION`.                                                                                                                                                                                                                                                                                                                           |
| > pendingCloseOrdLiabVal | String | The amount of close orders of isolated margin liability.                                                                                                                                                                                                                                                                                                             |
| > adl                    | String | Automatic-Deleveraging, signal area<br>Divided into 6 levels, from 0 to 5, the smaller the number, the weaker the adl intensity. <br/>Only applicable to `FUTURES/SWAP/OPTION`                                                                                                                                                                                                                                        |
| > bizRefId               | String | External business id, e.g. experience coupon id                                                                                                                                                                                                                                                                                                                      |
| > bizRefType             | String | External business type                                                                                                                                                                                                                                                                                                                                               |
| > ccy                    | String | Currency used for margin                                                                                                                                                                                                                                                                                                                                             |
| > last                   | String | Latest traded price                                                                                                                                                                                                                                                                                                                                                  |
| > idxPx                  | String | Latest underlying index price                                                                                                                                                                                                                                                                                                                                        |
| > usdPx                  | String   | Latest USD price of the `ccy` on the market, only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                                                                             |
| > bePx                   | String | Breakeven price                                                                                                                                                                                                                                                                                                                                                      |
| > deltaBS                | String | delta: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                  |
| > deltaPA                | String | delta: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                  |
| > gammaBS                | String | gamma: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                  |
| > gammaPA                | String | gamma: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                  |
| > thetaBS                | String | theta: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                  |
| > thetaPA                | String | theta: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                  |
| > vegaBS                 | String | vega: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                   |
| > vegaPA                 | String | vega: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                   |
| > spotInUseAmt           | String | Spot in use amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                               |
| > spotInUseCcy           | String | Spot in use unit, e.g. `BTC`<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                     |
| > clSpotInUseAmt         | String | User-defined spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                             |
| > maxSpotInUseAmt        | String | Max possible spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                             |
| > realizedPnl            | String | Realized profit and loss<br/>Only applicable to `FUTURES`/`SWAP`/`OPTION`<br/> realizedPnl=pnl+fee+fundingFee+liqPenalty+settledPnl                                                                                                                                                                                                                                             |
| > pnl                    | String | Accumulated pnl of closing order(s) (excluding the fee).                                                                                                                                                                                                                                                                                                                                  |
| > fee                    | String | Accumulated fee<br>Negative number represents the user transaction fee charged by the platform.Positive number represents rebate.                                                                                                                                                                                                                                    |
| > fundingFee             | String | Accumulated funding fee                                                                                                                                                                                                                                                                                                                                              |
| > liqPenalty             | String | Accumulated liquidation penalty. It is negative when there is a value.                                                                                                                                                                                                                                                                                               |
| > closeOrderAlgo         | Array of objects | Close position algo orders attached to the position. This array will have values only after you request "Place algo order" with `closeFraction`=1.                                                                                                                                                                                                                   |
| >> algoId                | String | Algo ID                                                                                                                                                                                                                                                                                                                                                              |
| >> slTriggerPx           | String | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                             |
| >> slTriggerPxType       | String | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                   |
| >> tpTriggerPx           | String | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                           |
| >> tpTriggerPxType       | String | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                 |
| >> closeFraction         | String | Fraction of position to be closed when the algo order is triggered.                                                                                                                                                                                                                                                                                                  |
| > cTime                  | String | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                                                                                                                                                                                                                                                                          |
| > uTime                  | String | Latest time position was adjusted, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                                                                                                                                                                                                                                                      |
| > pTime                  | String | Push time of positions information, Unix timestamp format in milliseconds, e.g. `1597026383085`.                                                                                                                                                                                                                                                                     |
| > nonSettleAvgPx         | String    | Non-Settlement entry price<br>The non-settlement entry price only reflects the average price at which the position is opened or increased.<br>Applicable to `FUTURES` `cross`                                                                                                                                                                                        |
| > settledPnl             | String    | Accumulated settled P&L (calculated by settlement price)<br>Applicable to `FUTURES` `cross`                                                                                                                                                                                                                                                                          |

<aside class="notice"> 
<br> - The position data is sent on event basis and regular basis
<br> - The event push is not pushed in real-time. It is aggregated and pushed at a fixed time interval, around 50ms. For example, if multiple events occur within a fixed time interval, the system will aggregate them into a single message and push it at the end of the fixed time interval. If the data volume is too large, it may be split into multiple messages.
<br> - The regular push sends updates regardless of whether there are position activities or not.
<br> - If an event push and a regular push happen at the same time, the system will send the event push first, followed by the regular push.
</aside>

<aside class="notice">
As for portfolio margin account, the IMR and MMR of the position are calculated in risk unit granularity, thus their values of the same risk unit cross positions are the same.
</aside>

<aside class="notice">
In the position-by-position trading setting, it is an autonomous transfer mode. After the margin is transferred, positions with a position of 0 will be pushed
</aside>

<aside class="notice"> 
<br> - Only position with non-zero position quantity will be pushed. Definition of non-zero quantity: value of pos parameter is not 0. If the data is too large to be sent in a single push message, it will be split into multiple messages.
<br> - For example, when subscribing to positions channel specifying an underlying and there are 20 positions are with non-zero quantity, all 20 positions data will be pushed in initial snapshot and in regular push. Subsequently when there is change in pos of a position, only the data of that position will be pushed triggered by this change.
</aside>

<aside class="notice">
Unlike futures contracts, option positions are automatically exercised or expire at maturity. The rights then terminate and no closing orders are generated. Therefore, this channel will not push any updates for expired option positions.
</aside>
