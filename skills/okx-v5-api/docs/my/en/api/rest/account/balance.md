---
site: my
lang: en
name: Get balance
method: GET
path: /api/v5/account/balance
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get balance

Retrieve a list of assets (with non-zero balance), remaining balance, and available amount in the trading account.

<aside class="notice">
Interest-free quota and discount rates are public data and not displayed on the account interface.
</aside>

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/balance`

> Request Example

```shell
# Get the balance of all assets in the account
GET /api/v5/account/balance

# Get the balance of BTC and ETH assets in the account
GET /api/v5/account/balance?ccy=BTC,ETH

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get account balance
result = accountAPI.get_account_balance()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                         |
| :------------- | :-------- | :----------- | :------------------------------------------------------------------------------------------------------ |
| ccy            | String    | No           | Single currency or multiple currencies (no more than 20) separated with comma, e.g. `BTC` or `BTC,ETH`. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "55415.624719833286",
            "availEq": "55415.624719833286",
            "borrowFroz": "0",
            "delta": "0",
            "deltaLever": "0",
            "deltaNeutralStatus": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "availBal": "4834.317093622894",
                    "availEq": "4834.3170936228935",
                    "borrowFroz": "0",
                    "cashBal": "4850.435693622894",
                    "ccy": "USDT",
                    "crossLiab": "0",
                    "colRes": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "4991.542013297616",
                    "eq": "4992.890093622894",
                    "eqUsd": "4991.542013297616",
                    "smtSyncEq": "0",
                    "spotCopyTradingEq": "0",
                    "fixedBal": "0",
                    "frozenBal": "158.573",
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
                    "maxSpotInUse": "",
                    "spotIsoBal": "0",
                    "stgyEq": "150",
                    "twap": "0",
                    "uTime": "1705449605015",
                    "upl": "-7.545600000000006",
                    "uplLiab": "0",
                    "spotBal": "",
                    "openAvgPx": "",
                    "accAvgPx": "",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "totalPnl": "",
                    "totalPnlRatio": ""
                }
            ],
            "imr": "0",
            "isoEq": "0",
            "mgnRatio": "",
            "mmr": "0",
            "notionalUsd": "0",
            "notionalUsdForBorrow": "0",
            "notionalUsdForFutures": "0",
            "notionalUsdForOption": "0",
            "notionalUsdForSwap": "0",
            "ordFroz": "",
            "totalEq": "55837.43556134779",
            "uTime": "1705474164160",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameters**        | **Types** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uTime                 | String    | Update time of account information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| totalEq               | String    | Total account assets denominated in `USD`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| isoEq                 | String    | Isolated margin equity in `USD`<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| adjEq                 | String    | Adjusted equity in `USD`: `totalEq` minus haircut discounts applied to non-stablecoin collateral assets. This is the operative value used in the margin ratio calculation (`mgnRatio` = `adjEq` / `mmr`). <br>The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode. <br>In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation. <br>Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks. <br>Applicable to `Spot mode`/`Multi-currency margin` and `Portfolio margin` |
| availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit.<br> Applicable to `Multi-currency margin`/`Portfolio margin` |
| ordFroz               | String    | Cross margin frozen for pending orders in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| imr                   | String    | Initial Margin Requirement (IMR) in `USD`: equity locked across all open cross-margin positions. The sum of initial margins of all open positions and pending orders under cross-margin mode. Formula per position: position size × markPx × initial margin rate (= 1/lever). Returns empty string in Simple mode. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                            |
| mmr                   | String    | Maintenance Margin Requirement (MMR) in `USD`: the minimum equity required to avoid forced liquidation. The sum of maintenance margins of all open positions and pending orders under cross-margin mode. When `adjEq` ≤ `mmr` (equivalently, `mgnRatio` ≤ 1.0), the system begins forced liquidation of positions. Subscribe to the position-risk-warning WebSocket channel for proactive alerts. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                    |
| borrowFroz            | String    | Potential borrowing IMR of the account in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| mgnRatio              | String    | Account-level margin ratio = `adjEq` / `mmr`. Values at or below 1.0 indicate the account is at or past the liquidation boundary. Monitor this field or subscribe to the position-risk-warning WebSocket channel for proactive alerts. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| notionalUsd           | String    | Gross notional value of all open derivative positions converted to USD. Linear contracts: sz × ctVal × markPx. Inverse contracts: sz × ctVal (USD-denominated face value). Gross = long and short are not netted. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| notionalUsdForBorrow  | String   | Notional value for `Borrow` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| notionalUsdForSwap    | String   | Notional value of positions for `Perpetual Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| notionalUsdForFutures | String   | Notional value of positions for `Expiry Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| notionalUsdForOption  | String   | Notional value of positions for `Option` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| upl                   | String    | Unrealized PnL across all open cross-margin positions at the account level, in `USD`. Calculated using mark price (not last trade price). Positive = unrealized gain; negative = unrealized loss. Returns empty string in Simple mode and Single-currency margin mode.<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| delta | String | Delta (USD) |
| deltaLever | String | Delta neutral strategy account level delta leverage<br>deltaLever = delta / totalEq |
| deltaNeutralStatus | String | Delta risk status<br>`0`: normal<br>`1`: transfer restricted<br>`2`: delta reducing - cancel all pending orders if delta is greater than 5000 USD, only one delta reducing order allowed per index (spot, futures, swap) |
| details               | Array of objects     | Detailed asset information in all currencies                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > ccy                 | String    | Currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > eq                  | String    | Equity of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > cashBal             | String    | Cash balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > uTime               | String    | Update time of currency balance information, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > isoEq               | String    | Isolated margin equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > availEq             | String    | Available equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > disEq               | String    | Discount equity of currency in `USD`.<br>Applicable to `Spot mode`(enabled spot borrow)/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > fixedBal            | String    | Frozen balance for `Dip Sniper` and `Peak Sniper`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > availBal            | String    | Available balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > frozenBal           | String    | Frozen balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > ordFrozen           | String    | Margin frozen for open orders<br>Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > liab                | String    | Liabilities of currency<br>It is a positive value, e.g. `21625.64`<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > upl                 | String    | The sum of the unrealized profit & loss of all margin and derivatives positions of currency. <br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > uplLiab             | String    | Liabilities due to Unrealized loss of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > crossLiab           | String    | Cross liabilities of currency<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > rewardBal           | String    | Trial fund balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > isoLiab             | String    | Isolated liabilities of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > mgnRatio            | String    | Cross maintenance margin ratio of currency <br>The index for measuring the risk of a certain asset in the account. <br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                              |
| > imr                 | String    | Cross initial margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > mmr                 | String    | Cross maintenance margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > interest            | String    | Accrued interest of currency<br>It is a positive value, e.g. `9.01`<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > twap                | String    | Risk indicator of forced repayment<br>Divided into multiple levels from 0 to 5, the larger the number, the more likely the forced repayment will be triggered. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                     |
| > frpType | String | Forced repayment (FRP) type<br> `0`: no FRP<br> `1`: user based FRP<br> `2`: platform based FRP<br><br> Return `1`/`2` when twap is >= 1, applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`|
| > maxLoan             | String    | Maximum borrowable amount for the currency under the current account conditions. Affects the amount available for margin borrowing and transfers.<br>Applicable to `cross` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                              |
| > eqUsd               | String    | Equity in `USD` of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > borrowFroz          | String    | Potential borrowing IMR of currency in `USD` <br>Applicable to `Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > notionalLever       | String    | Leverage of currency<br>Applicable to `Futures mode`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > stgyEq              | String    | Total equity allocated to trading bots for the currency. Covers Spot Grid, Futures Grid, Signal Bot, Futures Martingale, Spot Martingale, Infinite Grid, and Recurring Buy strategies.                                                                                                                                                                                                                                                                                                                                                                                                            |
| > isoUpl              | String    | Isolated unrealized profit and loss of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > spotInUseAmt        | String    | Actual spot hedging amount in use for the currency. <br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                   |
| > clSpotInUseAmt      | String | User-defined spot hedging amount for the currency. Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                            |
| > maxSpotInUse        | String | System-calculated maximum possible spot hedging amount for the currency.<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > spotIsoBal          | String    | Balance acquired through spot copy trading (as a follower or lead trader), including amounts currently frozen by open orders. For example, if 1 BTC was purchased via copy trading and 0.4 BTC is frozen in an open sell order, `spotIsoBal` returns `1`, not `0.6`.<br>Applicable to copy trading. Applicable to `Spot mode`/`Futures mode`.                                                                                                                                                                                                                                                     |
| > smtSyncEq           | String     | Smart sync equity<br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > spotCopyTradingEq   | String | Spot smart sync equity. <br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > spotBal             | String | Spot balance. The unit is currency, e.g. BTC. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > openAvgPx           | String | Spot average cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > accAvgPx            | String | Spot accumulated cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > spotUpl             | String | Spot unrealized profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > spotUplRatio        | String | Spot unrealized profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > totalPnl            | String | Spot accumulated profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > totalPnlRatio       | String | Spot accumulated profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > colRes               | String          | Platform level collateral restriction status<br> `0`: The restriction is not enabled.<br> `1`: The restriction is not enabled. But the crypto is close to the platform's collateral limit.<br> `2`: The restriction is enabled. This crypto can't be used as margin for your new orders. This may result in failed orders. But it will still be included in the account's adjusted equity and doesn't impact margin ratio.<br> Refer to [Introduction to the platform collateralized borrowing limit](https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details. |
| > colBorrAutoConversion | String         | Risk indicator of auto conversion. Divided into multiple levels from 1-5, the larger the number, the more likely the repayment will be triggered. The default will be 0, indicating there is no risk currently. 5 means this user is undergoing auto conversion now, 4 means this user will undergo auto conversion soon whereas 1/2/3 indicates there is a risk for auto conversion.<br> Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`/`Portfolio margin`<br> When the total liability for each crypto set as collateral exceeds a certain percentage of the platform's total limit, the auto-conversion mechanism may be triggered. This may result in the automatic sale of excess collateral crypto if you've set this crypto as collateral and have large borrowings. To lower this risk, consider reducing your use of the crypto as collateral or reducing your liabilities.<br> Refer to [Introduction to the platform collateralized borrowing limit](https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details.|
| > collateralRestrict | Boolean | ~~Platform level collateralized borrow restriction<br> `true`<br> `false`~~(deprecated, use colRes instead) |
| > collateralEnabled    | Boolean   | `true`: Collateral enabled<br>`false`: Collateral disabled<br>Applicable to `Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                   |
| > autoLendStatus | String | Auto lend status<br> `unsupported`: auto lend is not supported by this currency<br> `off`: auto lend is supported but turned off<br> `pending`: auto lend is turned on but pending matching<br> `active`: auto lend is turned on and matched |
| > autoLendMtAmt | String | Auto lend currency matched amount<br> Return "0" when autoLendStatus is `unsupported/off/pending`. Return matched amount when autoLendStatus is `active` |

* Regarding more parameter details, you can refer to product documentations below:<br>
[Futures mode: cross margin trading](https://www.okx.com/help/iii-single-currency-margin-cross-margin-trading) <br>
[Multi-currency margin mode: cross margin trading](https://www.okx.com/help/iv-multi-currency-margin-mode-cross-margin-trading) <br>
[Multi-currency margin mode vs. Portfolio margin mode](https://www.okx.com/help/vi-multi-currency-margin-mode-vs-portfolio-margin-mode) <br>

<aside class="notice">
"" will be returned for inapplicable fields under the current account level.
</aside>

<aside class="notice">
The currency details will not be returned when cashBal and eq is both 0.
</aside>

Distribution of applicable fields under each account level are as follows:

| **Parameters**          | **Spot mode** | **Futures mode** | **Multi-currency margin mode** | **Portfolio margin mode** |
|:------------------------|:--------------|:--------------------------|:-------------------------------|:--------------------------|
| uTime                   | Yes           | Yes                       | Yes                            | Yes                       |
| totalEq                 | Yes           | Yes                       | Yes                            | Yes                       |
| isoEq                   |               | Yes                       | Yes                            | Yes                       |
| adjEq                   | Yes           |                           | Yes                            | Yes                       |
| availEq                   |            |                           | Yes                            | Yes                       |
| ordFroz	                | Yes           |                           | Yes                            | Yes                       |
| imr                     | Yes           |                           | Yes                            | Yes                       |
| mmr                     | Yes           |                           | Yes                            | Yes                       |
| borrowFroz              | Yes           |                           | Yes                            | Yes                       |
| mgnRatio                | Yes           |                           | Yes                            | Yes                       |
| notionalUsd             | Yes           |                           | Yes                            | Yes                       |
| notionalUsdForSwap      |               |                           | Yes                                    | Yes                       |
| notionalUsdForFutures   |               |                           | Yes                                    | Yes                       |
| notionalUsdForOption    | Yes           |                           | Yes                                    | Yes                       |
| notionalUsdForBorrow    | Yes           |                           | Yes                                    | Yes                       |
| upl                     |               |                           | Yes                            | Yes                       |
| details                 | Yes           | Yes                       | Yes                            | Yes                       |
| > ccy                   | Yes           | Yes                       | Yes                            | Yes                       |
| > eq                    | Yes           | Yes                       | Yes                            | Yes                       |
| > cashBal               | Yes           | Yes                       | Yes                            | Yes                       |
| > uTime                 | Yes           | Yes                       | Yes                            | Yes                       |
| > isoEq                 |               | Yes                       | Yes                            | Yes                       |
| > availEq               |               | Yes                       | Yes                            | Yes                       |
| > disEq                 | Yes           |                           | Yes                            | Yes                       |
| > availBal              | Yes           | Yes                       | Yes                            | Yes                       |
| > frozenBal             | Yes           | Yes                       | Yes                            | Yes                       |
| > ordFrozen             | Yes           | Yes                       | Yes                            | Yes                       |
| > liab                  | Yes           |                           | Yes                            | Yes                       |
| > upl                   |               | Yes                       | Yes                            | Yes                       |
| > uplLiab               |               |                           | Yes                            | Yes                       |
| > crossLiab             | Yes           |                           | Yes                            | Yes                       |
| > isoLiab               |               |                           | Yes                            | Yes                       |
| > mgnRatio              |               | Yes                       |                                |                           |
| > interest              | Yes           |                           | Yes                            | Yes                       |
| > twap                  | Yes           |                           | Yes                            | Yes                       |
| > maxLoan               | Yes           |                           | Yes                            | Yes                       |
| > eqUsd                 | Yes           | Yes                       | Yes                            | Yes                       |
| > borrowFroz            | Yes           |                           | Yes                            | Yes                       |
| > notionalLever         |               | Yes                       |                                |                           |
| > stgyEq                | Yes           | Yes                       | Yes                            | Yes                       |
| > isoUpl                |               | Yes                       | Yes                            | Yes                       |
| > spotInUseAmt          |               |                           |                                | Yes                       |        
| > clSpotInUseAmt        |               |                           |                                | Yes                       |        
| > maxSpotInUse          |               |                           |                                | Yes                       |        
| > spotIsoBal            | Yes           | Yes                       |                                |                           |
| > imr                   |               | Yes                       |                                |                           |
| > mmr                   |               | Yes                       |                                |                           |
| > smtSyncEq             | Yes           | Yes                       | Yes                            | Yes                       |
| > spotCopyTradingEq     | Yes           | Yes                       | Yes                            | Yes                       |
| > spotBal               | Yes           | Yes                       | Yes                            | Yes                       |
| > openAvgPx             | Yes           | Yes                       | Yes                            | Yes                       |
| > accAvgPx              | Yes           | Yes                       | Yes                            | Yes                       |
| > spotUpl               | Yes           | Yes                       | Yes                            | Yes                       |
| > spotUplRatio          | Yes           | Yes                       | Yes                            | Yes                       |
| > totalPnl              | Yes           | Yes                       | Yes                            | Yes                       |
| > totalPnlRatio         | Yes           | Yes                       | Yes                            | Yes                       |
| > collateralEnabled     |               |                           | Yes                            |                           |
