---
site: my
lang: en
name: Get positions
method: GET
path: /api/v5/account/positions
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get positions

Retrieve information on your positions. When the account is in `net` mode, `net` positions will be displayed, and when the account is in `long/short` mode, `long` or `short` positions will be displayed. Return in reverse chronological order using ctime.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/positions`

> Request Example

```shell
# Query BTC-USDT position information
GET /api/v5/account/positions?instId=BTC-USDT

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get positions information
result = accountAPI.get_positions()
print(result)
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description**                                                                                                                                                                                                                          |
| :------------ | :------- | :----------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType      | String   | No           | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS`<br>`instId` will be checked against `instType` when both parameters are passed.                                                                                           |
| instId        | String   | No           | Instrument ID, e.g. `BTC-USDT-SWAP`. Single instrument ID or multiple instrument IDs (no more than 10) separated with comma                                                                                                              |
| posId         | String   | No           | Single position ID or multiple position IDs (no more than 20) separated with comma. <br>There is attribute expiration, the posId and position information will be cleared if it is more than 30 days after the last full close position. |

<aside class="notice">
instId<br>
If the instrument ever had position and its open interest is 0, it will return the position information with specific instId. It will not return the position information with specific instId if there is no valid posId; it will not return the position information without specific instId. 
</aside>

<aside class="notice">
In the isolated margin trading settings, if it is set to the manual transfers mode, after the position is transferred to the margin, a position with a position of 0 will be generated
</aside>

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "availPos": "0.00190433573",
            "avgPx": "62961.4",
            "baseBal": "",
            "baseBorrowed": "",
            "baseInterest": "",
            "bePx": "",
            "bizRefId": "",
            "bizRefType": "",
            "cTime": "1724740225685",
            "ccy": "BTC",
            "clSpotInUseAmt": "",
            "closeOrderAlgo": [],
            "deltaBS": "",
            "deltaPA": "",
            "fee": "",
            "fundingFee": "",
            "gammaBS": "",
            "gammaPA": "",
            "hedgedPos": "",
            "idxPx": "62890.5",
            "imr": "",
            "instId": "BTC-USDT",
            "instType": "MARGIN",
            "interest": "0",
            "last": "62892.9",
            "lever": "5",
            "liab": "-99.9998177776581948",
            "liabCcy": "USDT",
            "liqPenalty": "",
            "liqPx": "53615.448336593756",
            "margin": "0.000317654",
            "markPx": "62891.9",
            "maxSpotInUseAmt": "",
            "mgnMode": "isolated",
            "mgnRatio": "9.404143929947395",
            "mmr": "0.0000318005395854",
            "notionalUsd": "119.756628017499",
            "optVal": "",
            "pendingCloseOrdLiabVal": "0",
            "pnl": "",
            "pos": "0.00190433573",
            "posCcy": "BTC",
            "posId": "1752810569801498626",
            "posSide": "net",
            "quoteBal": "",
            "quoteBorrowed": "",
            "quoteInterest": "",
            "realizedPnl": "",
            "spotInUseAmt": "",
            "spotInUseCcy": "",
            "thetaBS": "",
            "thetaPA": "",
            "tradeId": "785524470",
            "uTime": "1724742632153",
            "upl": "-0.0000033452492717",
            "uplLastPx": "-0.0000033199677697",
            "uplRatio": "-0.0105311101755551",
            "uplRatioLastPx": "-0.0104515220008934",
            "usdPx": "",
            "vegaBS": "",
            "vegaPA": "",
            "nonSettleAvgPx":"",
            "settledPnl":""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**            | **Type**        | **Description**                                                                                                                                                                                                                                                                                                                                                      |
|:-------------------------|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType                 | String          | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| mgnMode                  | String          | Margin mode<br>`cross`  <br>`isolated`                                                                                                                                                                                                                                                                                                                               |
| posId                    | String          | Position ID                                                                                                                                                                                                                                                                                                                                                          |
| posSide                  | String          | Position side<br>`long`, `pos` is positive  <br>`short`, `pos` is positive  <br>`net` (`FUTURES`/`SWAP`/`OPTION`: positive `pos` means long position and negative `pos` means short position. For `MARGIN`, `pos` is always positive, `posCcy` being base currency means long position, `posCcy` being quote currency means short position.)                         |
| pos                      | String          | Position quantity. Unit: number of contracts for SWAP/FUTURES/OPTIONS; base currency amount for MARGIN. Sign (net mode): positive = long, negative = short. In long/short mode, separate records are returned per side — check `posSide`. In the isolated margin mode, when doing manual transfers, a position with pos of `0` will be generated after the deposit is transferred (represents a funded-but-empty position record created after a margin deposit).                                                                                                                                                                                                       |
| hedgedPos | String | Hedged position size<br>Only return for accounts in delta neutral strategy, stgyType:1. Return "" for accounts in general strategy. |
| baseBal                  | String          | ~~Base currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                                |
| quoteBal                 | String          | ~~Quote currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                               |
| baseBorrowed             | String          | ~~Base currency amount already borrowed, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                  |
| baseInterest             | String          | ~~Base Interest, undeducted interest that has been incurred, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                              |
| quoteBorrowed            | String          | ~~Quote currency amount already borrowed, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                                                 |
| quoteInterest            | String          | ~~Quote Interest, undeducted interest that has been incurred, only applicable to MARGIN(Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                                                                             |
| posCcy                   | String          | Position currency, only applicable to `MARGIN` positions.                                                                                                                                                                                                                                                                                                            |
| availPos                 | String          | Position that can be closed <br>Only applicable to `MARGIN` and `OPTION`.<br>For `MARGIN` position, the rest of sz will be `SPOT` trading after the liability is repaid while closing the position. Please get the available reduce-only amount from "Get maximum available tradable amount" if you want to reduce the amount of `SPOT` trading as much as possible. |
| avgPx                    | String          | Volume-weighted average entry price of the current open position. Denominated in quote currency for linear contracts (e.g., USDT for BTC-USDT-SWAP) and in USD for inverse contracts (e.g., BTC-USD-SWAP). Recalculated after each fill that changes position size.<br>Under cross-margin mode, the entry price of expiry futures will update at settlement to the last settlement price, and when the position is opened or increased.                                                                                                                                                                               |
| nonSettleAvgPx           | String          | Non-settlement entry price<br>The non-settlement entry price only reflects the average price at which the position is opened or increased.<br>Applicable to `cross` `FUTURES` positions.                                                                                                                                                                             |
| markPx                   | String          | Latest Mark price                                                                                                                                                                                                                                                                                                                                                    |
| upl                      | String          | Unrealized PnL for this position, denominated in the instrument's settlement currency (see `ccy`). Formula: (markPx − avgPx) × pos × ctVal for linear; (1/avgPx − 1/markPx) × pos × ctVal for inverse. For account-level USD total, see `upl` in GET /api/v5/account/balance.                                                                                                                                                                                                                                                                                                                                                                                         |
| uplRatio                 | String          | Unrealized profit and loss ratio calculated by mark price.                                                                                                                                                                                                                                                                                                           |
| uplLastPx                | String          | Unrealized profit and loss calculated by last price. Main usage is showing, actual value is upl.                                                                                                                                                                                                                                                                     |
| uplRatioLastPx           | String          | Unrealized profit and loss ratio calculated by last price. 	                                                                                                                                                                                                                                                                                                         |
| instId                   | String          | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                                                                                                                  |
| lever                    | String          | Leverage<br> Not applicable to `OPTION` and positions of cross margin mode under `Portfolio margin`                                                                                                                                                                                                                                                                  |
| liqPx                    | String          | Estimated mark price at which this position would be forcibly liquidated. This is an estimate based on current equity and margin rates — the actual liquidation price can change quickly due to funding rate accrual, other position changes, or rapid market moves.  <br>Not applicable to `OPTION`                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| imr                      | String          | Initial margin requirement for this specific cross-margin position, in USD. Formula: position size × markPx × initial margin rate (1/lever). For total account IMR, see `imr` in GET /api/v5/account/balance. Empty string for isolated positions. Only applicable to `cross`.                                                                                                                                                                                                                                                                                                              |
| margin                   | String          | Margin, can be added or reduced. Only applicable to `isolated`.                                                                                                                                                                                                                                                                                                      |
| mgnRatio                 | String          | Maintenance margin ratio                                                                                                                                                                                                                                                                                                                                             |
| mmr                      | String          | Maintenance margin requirement                                                                                                                                                                                                                                                                                                                                       |
| liab                     | String          | Liabilities, only applicable to `MARGIN`.                                                                                                                                                                                                                                                                                                                            |
| liabCcy                  | String          | Liabilities currency, only applicable to `MARGIN`.                                                                                                                                                                                                                                                                                                                   |
| interest                 | String          | Interest. Undeducted interest that has been incurred.                                                                                                                                                                                                                                                                                                                |
| tradeId                  | String          | Last trade ID                                                                                                                                                                                                                                                                                                                                                        |
| optVal                   | String          | Option Value, only applicable to `OPTION`.                                                                                                                                                                                                                                                                                                                           |
| pendingCloseOrdLiabVal   | String          | The amount of close orders of isolated margin liability.                                                                                                                                                                                                                                                                                                             |
| notionalUsd              | String          | Notional value of positions in `USD`                                                                                                                                                                                                                                                                                                                                 |
| adl                      | String          | Auto-Deleveraging (ADL) indicator. Range: 0–5, where 0 = lowest ADL priority (least likely to be forcibly deleveraged) and 5 = highest priority (first in queue if the insurance fund is depleted). Priority increases with higher unrealized profit and higher leverage. <br/>Only applicable to `FUTURES/SWAP/OPTION`                                                                                                                                                                                         |
| ccy                      | String          | Currency used for margin                                                                                                                                                                                                                                                                                                                                             |
| last                     | String          | Latest traded price                                                                                                                                                                                                                                                                                                                                                  |
| idxPx                    | String          | Latest underlying index price                                                                                                                                                                                                                                                                                                                                        |
| usdPx                    | String          | Latest USD price of the `ccy` on the market, only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                                                                             |
| bePx                     | String          | Breakeven price                                                                                                                                                                                                                                                                                                                                                      |
| deltaBS                  | String          | delta: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                  |
| deltaPA                  | String          | delta: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                  |
| gammaBS                  | String          | gamma: Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                  |
| gammaPA                  | String          | gamma: Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                  |
| thetaBS                  | String          | theta：Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                   |
| thetaPA                  | String          | theta：Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                   |
| vegaBS                   | String          | vega：Black-Scholes Greeks in dollars, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                    |
| vegaPA                   | String          | vega：Greeks in coins, only applicable to `OPTION`                                                                                                                                                                                                                                                                                                                    |
| spotInUseAmt             | String          | Spot in use amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                               |
| spotInUseCcy             | String          | Spot in use unit, e.g. `BTC`<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                     |
| clSpotInUseAmt           | String          | User-defined spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                             |
| maxSpotInUseAmt          | String          | Max possible spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                             |
| bizRefId                 | String          | External business id, e.g. experience coupon id                                                                                                                                                                                                                                                                                                                      |
| bizRefType               | String          | External business type                                                                                                                                                                                                                                                                                                                                               |
| realizedPnl              | String          | Realized profit and loss<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`<br>`realizedPnl`=`pnl`+`fee`+`fundingFee`+`liqPenalty`+`settledPnl`                                                                                                                                                                                                                         |
| settledPnl               | String          | Accumulated settled profit and loss (calculated by settlement price)<br>Only applicable to `cross` `FUTURES`                                                                                                                                                                                                                                                         |
| pnl                      | String          | Accumulated pnl of closing order(s) (excluding the fee).                                                                                                                                                                                                                                                                                                                                  |
| fee                      | String          | Accumulated fee since the current position was opened. Resets to 0 when the position is fully closed. For per-fill fees, use GET /api/v5/trade/fills.<br>Negative number represents the user transaction fee charged by the platform. Positive number represents rebate. |
| fundingFee               | String          | Accumulated funding fee                                                                                                                                                                                                                                                                                                                                              |
| liqPenalty               | String          | Accumulated liquidation penalty. It is negative when there is a value.                                                                                                                                                                                                                                                                                               |
| closeOrderAlgo           | Array of objects | Close position algo orders attached to the position. This array will have values only after you request "Place algo order" with `closeFraction`=1.                                                                                                                                                                                                                   |
| > algoId                 | String          | Algo ID                                                                                                                                                                                                                                                                                                                                                              |
| > slTriggerPx            | String          | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                             |
| > slTriggerPxType        | String          | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                   |
| > tpTriggerPx            | String          | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                           |
| > tpTriggerPxType        | String          | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                 |
| > closeFraction          | String          | Fraction of position to be closed when the algo order is triggered.                                                                                                                                                                                                                                                                                                  |
| cTime                    | String          | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                           |
| uTime                    | String          | Latest time position was adjusted, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                       |

<aside class="notice">
<p>
As for portfolio margin account, the IMR and MMR of the position are calculated in risk unit granularity, thus their values of the same risk unit cross positions are the same.
</p>
</aside>
