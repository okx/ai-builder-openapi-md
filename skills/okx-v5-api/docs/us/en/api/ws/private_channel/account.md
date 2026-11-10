---
site: us
lang: en
name: WebSocket
type: guide
---

## WebSocket
### Account channel

Retrieve account information. Data will be pushed when triggered by events such as placing order, canceling order, transaction execution, etc.
It will also be pushed in regular interval according to subscription granularity.<br>

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
      "channel": "account",
      "ccy": "BTC"
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
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "account",
          "ccy": "BTC"
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
      "channel": "account",
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
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "account",
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

| Parameter | Type   | Required | Description                          |
| :-------- | :----- | :------- | ------------------------------------ |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args    | Array of objects  | Yes      | List of subscribed channels          |
| > channel | String | Yes      | Channel name<br>`account`              |
| > ccy     | String | No       | Currency                             |
| > extraParams | String | No | Additional configuration |
| >> updateInterval | int | No | `0`: only push due to account events <br>The data will be pushed both by events and regularly if this field is omitted or set to other values than 0. <br>The following format should be strictly obeyed when using this field. <br>"extraParams": "<br>{<br>   \"updateInterval\": \"0\"<br> }<br>"|

> Successful Response Example : single

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "account",
    "ccy": "BTC"
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
    "channel": "account"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type     | Required | Description                                  |
|:----------|:---------|:---------|----------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String   | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | No       | Subscribed channel                           |
| > channel | String   | Yes      | Channel name<br>`account`                      |
| > ccy     | String   | No       | Currency                                     |
| code      | String   | No       | Error code                                   |
| msg       | String   | No       | Error message                                |
| connId    | String   | Yes      | WebSocket connection ID                      |

> Push Data Example

```json
{
	"arg": {
		"channel": "account",
		"uid": "44*********584"
	},
	"eventType": "snapshot",
	"curPage": 1,
	"lastPage": true,
	"data": [{
		"adjEq": "55444.12216906034",
    "availEq": "55444.12216906034",
		"borrowFroz": "0",
    "delta": "0",
    "deltaLever": "0",
    "deltaNeutralStatus": "0",
		"details": [{
        "availBal": "4734.371190691436",
        "availEq": "4734.371190691435",
        "borrowFroz": "0",
        "cashBal": "4750.426970691436",
        "ccy": "USDT",
        "coinUsdPrice": "0.99927",
        "crossLiab": "0",
        "colRes": "0",
        "collateralEnabled": false,
        "collateralRestrict": false, // Deprecated, use colRes instead
        "colBorrAutoConversion": "0",
        "disEq": "4889.379316336831",
        "eq": "4892.951170691435",
        "eqUsd": "4889.379316336831",
        "smtSyncEq": "0",
        "spotCopyTradingEq": "0",
        "fixedBal": "0",
        "frozenBal": "158.57998",
        "frpType": "0",
        "imr": "",
        "interest": "0",
        "isoEq": "0",
        "isoLiab": "0",
        "isoUpl": "0",
        "liab": "0",
        "maxLoan": "0",
        "mgnRatio": "",
        "mmr": "",
        "notionalLever": "",
        "ordFrozen": "0",
        "rewardBal": "0",
        "spotInUseAmt": "",
        "clSpotInUseAmt": "",
        "maxSpotInUseAmt": "",			
        "spotIsoBal": "0",
        "stgyEq": "150",
        "twap": "0",
        "uTime": "1705564213903",
        "upl": "-7.475800000000003",
        "uplLiab": "0",
        "spotBal": "",
        "openAvgPx": "",
        "accAvgPx": "",
        "spotUpl": "",
        "spotUplRatio": "",
        "totalPnl": "",
        "totalPnlRatio": ""
		}],
		"imr": "0",
		"isoEq": "0",
		"mgnRatio": "",
		"mmr": "0",
		"notionalUsd": "0",
        "notionalUsdForBorrow": "0",
        "notionalUsdForFutures": "0",
        "notionalUsdForOption": "0",
        "notionalUsdForSwap": "0",
		"ordFroz": "0",
		"totalEq": "55868.06403501676",
		"uTime": "1705564223311",
		"upl": "0"
	}]
}
```

#### Push data parameters

| **Parameters**          | **Types**  | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|-------------------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                     | Object     | Successfully subscribed channel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > channel               | String     | Channel name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > uid                   | String     | User Identifier                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| eventType               | String     | Event type: <br>`snapshot`: Initial and regular snapshot push <br>`event_update`: Event-driven update push                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| curPage                 | Integer    | Current page number. <br>Only applicable for `snapshot` events. Not included in `event_update` events.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| lastPage                | Boolean    | Whether this is the last page of pagination:<br>`true`<br>`false`<br>Only applicable for `snapshot` events. Not included in `event_update` events.                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| data                    | Array of objects      | Subscribed data                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > uTime                 | String     | The latest time to get account information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > totalEq               | String     | The total amount of equity in `USD`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > isoEq                 | String     | Isolated margin equity in `USD`<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > adjEq                 | String     | Adjusted / Effective equity in `USD` <br>The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode. <br>In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation. <br>Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin` |
| > availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit.<br> Applicable to `Multi-currency margin`/`Portfolio margin` |
| > ordFroz               | String     | Margin frozen for pending cross orders in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > imr                   | String     | Initial margin requirement in `USD` <br>The sum of initial margins of all open positions and pending orders under cross-margin mode in `USD`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                        |
| > mmr                   | String     | Maintenance margin requirement in `USD` <br>The sum of maintenance margins of all open positions and pending orders under cross-margin mode in `USD`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                |
| > borrowFroz            | String     | Potential borrowing IMR of the account in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > mgnRatio              | String     | Maintenance margin ratio in `USD`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > notionalUsd           | String     | Notional value of positions in `USD` <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > notionalUsdForBorrow  | String     | Notional value for `Borrow` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                  |
| > notionalUsdForSwap    | String     | Notional value of positions for `Perpetual Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                      |
| > notionalUsdForFutures | String     | Notional value of positions for `Expiry Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                         |
| > notionalUsdForOption  | String     | Notional value of positions for `Option` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                     |
| > upl                   | String     | Cross-margin info of unrealized profit and loss at the account level in `USD`<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > delta | String | Delta (USD) |
| > deltaLever | String | Delta neutral strategy account level delta leverage<br>deltaLever = delta / totalEq |
| > deltaNeutralStatus | String | Delta risk status<br>`0`: normal<br>`1`: transfer restricted<br>`2`: delta reducing - cancel all pending orders if delta is greater than 5000 USD, only one delta reducing order allowed per index (spot, futures, swap) |
| > details               | Array of objects      | Detailed asset information in all currencies                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> ccy                  | String     | Currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> eq                   | String     | Equity of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| >> cashBal              | String     | Cash balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> uTime                | String     | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> isoEq                | String     | Isolated margin equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| >> availEq              | String     | Available equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| >> disEq                | String     | Discount equity of currency in `USD`.<br>Applicable to `Spot mode`(enabled spot borrow)/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> fixedBal             | String     | Frozen balance for `Dip Sniper` and `Peak Sniper`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| >> availBal             | String     | Available balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| >> frozenBal            | String     | Frozen balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> ordFrozen            | String     | Margin frozen for open orders <br>Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| >> liab                 | String     | Liabilities of currency<br>It is a positive value, e.g. `21625.64`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> upl                  | String     | The sum of the unrealized profit & loss of all margin and derivatives positions of currency. <br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                             |
| >> uplLiab              | String     | Liabilities due to Unrealized loss of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> crossLiab            | String     | Cross liabilities of currency<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| >> isoLiab              | String     | Isolated liabilities of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> rewardBal            | String     | Trial fund balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| >> mgnRatio             | String     | Cross maintenance margin ratio of currency <br>The index for measuring the risk of a certain asset in the account. <br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                             |
| >> imr                  | String     | Cross initial margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| >> mmr                  | String     | Cross maintenance margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> interest             | String     | Interest of currency<br>It is a positive value, e.g."9.01". Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| >> twap                | String    | Risk indicator of forced repayment<br>Divided into multiple levels from 0 to 5, the larger the number, the more likely the forced repayment will be triggered. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                     |
| >> frpType | String | Forced repayment (FRP) type<br> `0`: no FRP<br> `1`: user based FRP<br> `2`: platform based FRP<br><br> Return `1`/`2` when twap is >= 1, applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`|
| >> maxLoan              | String     | Maximum borrowable amount for the currency under the current account conditions. Affects the amount available for margin borrowing and transfers.<br>Applicable to `cross` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                          |
| >> eqUsd                | String     | Equity `USD` of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> borrowFroz           | String     | Potential borrowing IMR of currency in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> notionalLever        | String     | Leverage of currency<br>Applicable to `Futures mode`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| >> coinUsdPrice         | String     | Price index `USD` of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| >> stgyEq               | String     | Total equity allocated to trading bots for the currency. Covers Spot Grid, Futures Grid, Signal Bot, Futures Martingale, Spot Martingale, Infinite Grid, and Recurring Buy strategies.                                                                                                                                                                                                                                                                                                                                                                                                        |
| >> isoUpl               | String     | Isolated unrealized profit and loss of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| >> spotInUseAmt         | String     | Actual spot hedging amount in use for the currency.<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                |
| >> clSpotInUseAmt       | String     | User-defined spot hedging amount for the currency.<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                        |
| >> maxSpotInUseAmt      | String     | System-calculated maximum possible spot hedging amount for the currency.<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| >> spotIsoBal           | String    | Balance acquired through spot copy trading (as a follower or lead trader), including amounts currently frozen by open orders. For example, if 1 BTC was purchased via copy trading and 0.4 BTC is frozen in an open sell order, `spotIsoBal` returns `1`, not `0.6`.<br>Applicable to copy trading. Applicable to `Spot mode`/`Futures mode`.                                                                                                                                                                                                                                |
| >> smtSyncEq            | String     | Smart sync equity<br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> spotCopyTradingEq    | String     | Spot smart sync equity. <br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| >> spotBal              | String     | Spot balance. The unit is currency, e.g. BTC. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> openAvgPx            | String     | Spot average cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| >> accAvgPx             | String     | Spot accumulated cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| >> spotUpl              | String     | Spot unrealized profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| >> spotUplRatio         | String     | Spot unrealized profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| >> totalPnl             | String     | Spot accumulated profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| >> totalPnlRatio        | String     | Spot accumulated profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| >> colRes               | String          | Platform level collateral restriction status<br> `0`: The restriction is not enabled.<br> `1`: The restriction is not enabled. But the crypto is close to the platform's collateral limit.<br> `2`: The restriction is enabled. This crypto can't be used as margin for your new orders. This may result in failed orders. But it will still be included in the account's adjusted equity and doesn't impact margin ratio.<br> Refer to [Introduction to the platform collateralized borrowing limit](https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details. |
| >> colBorrAutoConversion | String         | Risk indicator of auto conversion. Divided into multiple levels from 1-5, the larger the number, the more likely the repayment will be triggered. The default will be 0, indicating there is no risk currently. 5 means this user is undergoing auto conversion now, 4 means this user will undergo auto conversion soon whereas 1/2/3 indicates there is a risk for auto conversion.<br> Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`/`Portfolio margin`<br> When the total liability for each crypto set as collateral exceeds a certain percentage of the platform's total limit, the auto-conversion mechanism may be triggered. This may result in the automatic sale of excess collateral crypto if you've set this crypto as collateral and have large borrowings. To lower this risk, consider reducing your use of the crypto as collateral or reducing your liabilities.<br> Refer to [Introduction to the platform collateralized borrowing limit](https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details.|
| >> collateralRestrict | Boolean | ~~Platform level collateralized borrow restriction<br> `true`<br> `false`~~(deprecated, use colRes instead) |
| >> collateralEnabled    | Boolean   | `true`: Collateral enabled<br>`false`: Collateral disabled<br>Applicable to `Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                   |
| >> autoLendStatus | String | Auto lend status<br> `unsupported`: auto lend is not supported by this currency<br> `off`: auto lend is supported but turned off<br> `pending`: auto lend is turned on but pending matching<br> `active`: auto lend is turned on and matched |
| >> autoLendMtAmt | String | Auto lend currency matched amount<br> Return "0" when autoLendStatus is `unsupported/off/pending`. Return matched amount when autoLendStatus is `active` |

<aside class="notice">
"" will be returned for inapplicable fields under the current account level.
</aside>

<aside class="notice"> 
<br> - The account data is sent on event basis and regular basis.
<br> - The event push is not pushed in real-time. It is aggregated and pushed at a fixed time interval, around 50ms. For example, if multiple events occur within a fixed time interval, the system will aggregate them into a single message and push it at the end of the fixed time interval. If the data volume is too large, it may be split into multiple messages.
<br> - The regular push sends updates regardless of whether there are activities in the trading account or not.
</aside>

<aside class="notice"> 
<br> - Only currencies with non-zero balance will be pushed. Definition of non-zero balance: any value of eq, availEq, availBal parameters is not 0. If the data is too large to be sent in a single push message, it will be split into multiple messages.
<br> - For example, when subscribing to account channel without specifying ccy and there are 5 currencies are with non-zero balance, all 5 currencies data will be pushed in initial snapshot and in regular update. Subsequently when there is change in balance or equity of an token, only the incremental data of that currency will be pushed triggered by this change.
</aside>
