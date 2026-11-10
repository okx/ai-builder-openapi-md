---
site: okex
lang: en
name: Get sub-account trading balance
method: GET
path: /api/v5/account/subaccount/balances
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get sub-account trading balance

Query detailed balance info of Trading Account of a sub-account via the master account (applies to master accounts only)

#### Rate limit：6 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/account/subaccount/balances`

> Request sample

```shell
GET /api/v5/account/subaccount/balances?subAcct=test1

```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Get sub-account trading balance
result = subAccountAPI.get_account_balance(
    subAcct="hahawang1"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                 |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | Yes     | Sub-account name  |

> Returned result

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "101.46752000000001",
            "availEq": "624719833286",
            "borrowFroz": "0",
            "delta": "0",
            "deltaLever": "0",
            "deltaNeutralStatus": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "accAvgPx": "",
                    "availBal": "101.5",
                    "availEq": "101.5",
                    "borrowFroz": "0",
                    "cashBal": "101.5",
                    "ccy": "USDT",
                    "clSpotInUseAmt": "",
                    "crossLiab": "0",
                    "colRes": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
                    "colBorrAutoConversion": "0",
                    "disEq": "101.46752000000001",
                    "eq": "101.5",
                    "eqUsd": "101.46752000000001",
                    "fixedBal": "0",
                    "frozenBal": "0",
                    "frpType": "0",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "1015.0000000000001",
                    "maxSpotInUse": "",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "openAvgPx": "",
                    "ordFrozen": "0",
                    "rewardBal": "",
                    "smtSyncEq": "0",
                    "spotBal": "",
                    "spotCopyTradingEq": "0",
                    "spotInUseAmt": "",
                    "spotIsoBal": "0",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "stgyEq": "0",
                    "totalPnl": "",
                    "totalPnlRatio": "",
                    "twap": "0",
                    "uTime": "1663854334734",
                    "upl": "0",
                    "uplLiab": "0"
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
            "ordFroz": "0",
            "totalEq": "101.46752000000001",
            "uTime": "1739332269934",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameters**        | **Types** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uTime                 | String    | Update time of account information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| totalEq               | String    | The total amount of equity in `USD`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| isoEq                 | String    | Isolated margin equity in `USD`<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| adjEq                 | String    | Adjusted / Effective equity in `USD` <br>The net fiat value of the assets in the account that can provide margins for spot, expiry futures, perpetual futures and options under the cross-margin mode. <br>In multi-ccy or PM mode, the asset and margin requirement will all be converted to USD value to process the order check or liquidation. <br>Due to the volatility of each currency market, our platform calculates the actual USD value of each currency based on discount rates to balance market risks. <br>Applicable to `Spot mode`/`Multi-currency margin` and `Portfolio margin` |
| availEq | String | Account level available equity, excluding currencies that are restricted due to the collateralized borrowing limit.<br> Applicable to `Multi-currency margin`/`Portfolio margin` |
| ordFroz               | String    | Cross margin frozen for pending orders in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| imr                   | String    | Initial margin requirement in `USD` <br>The sum of initial margins of all open positions and pending orders under cross-margin mode in `USD`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                            |
| mmr                   | String    | Maintenance margin requirement in `USD` <br>The sum of maintenance margins of all open positions and pending orders under cross-margin mode in `USD`. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                    |
| borrowFroz            | String    | Potential borrowing IMR of the account in `USD` <br>Only applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| mgnRatio              | String    | Maintenance margin ratio in `USD` <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| notionalUsd           | String    | Notional value of positions in `USD` <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| notionalUsdForBorrow  | String   | Notional value for `Borrow` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                  |
| notionalUsdForSwap    | String   | Notional value of positions for `Perpetual Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                      |
| notionalUsdForFutures | String   | Notional value of positions for `Expiry Futures` in USD<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                         |
| notionalUsdForOption  | String   | Notional value of positions for `Option` in USD<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                     |
| upl                   | String    | Cross-margin info of unrealized profit and loss at the account level in `USD`<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| delta | String | Delta (USD) |
| deltaLever | String | Delta neutral strategy account level delta leverage<br>deltaLever = delta / totalEq |
| deltaNeutralStatus | String | Delta risk status<br>`0`: normal<br>`1`: transfer restricted<br>`2`: delta reducing - cancel all pending orders if delta is greater than 5000 USD, only one delta reducing order allowed per index (spot, futures, swap) |
| details               | Array of objects     | Detailed asset information in all currencies                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > ccy                 | String    | Currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > eq                  | String    | Equity of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > cashBal             | String    | Cash balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > uTime               | String    | Update time of currency balance information, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > isoEq               | String    | Isolated margin equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > availEq             | String    | Available equity of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > disEq               | String    | Discount equity of currency in `USD`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > fixedBal            | String    | Frozen balance for `Dip Sniper` and `Peak Sniper`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > availBal            | String    | Available balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > frozenBal           | String    | Frozen balance of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > ordFrozen           | String    | Margin frozen for open orders <br>Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > liab                | String    | Liabilities of currency<br>It is a positive value, e.g. `21625.64`<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > upl                 | String    | The sum of the unrealized profit & loss of all margin and derivatives positions of currency. <br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > uplLiab             | String    | Liabilities due to Unrealized loss of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > crossLiab           | String    | Cross liabilities of currency<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > rewardBal           | String    | Trial fund balance                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > isoLiab             | String    | Isolated liabilities of currency<br>Applicable to `Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > mgnRatio            | String    | Cross Maintenance margin ratio of currency <br>The index for measuring the risk of a certain asset in the account. <br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > imr                 | String    | Cross initial margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > mmr                 | String    | Cross maintenance margin requirement at the currency level<br>Applicable to `Futures mode` and when there is cross position                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > interest            | String    | Accrued interest of currency<br>It is a positive value, e.g. `9.01`<br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > twap                | String    | Risk indicator of forced repayment<br>Divided into multiple levels from 0 to 5, the larger the number, the more likely the forced repayment will be triggered. <br>Applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                     |
| > frpType | String | Forced repayment (FRP) type<br> `0`: no FRP<br> `1`: user based FRP<br> `2`: platform based FRP<br><br> Return `1`/`2` when twap is >= 1, applicable to `Spot mode`/`Multi-currency margin`/`Portfolio margin`|
| > maxLoan             | String    | Max loan of currency<br>Applicable to `cross` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > eqUsd               | String    | Equity in `USD` of currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > borrowFroz          | String    | Potential borrowing IMR of currency in `USD` <br>Applicable to `Multi-currency margin`/`Portfolio margin`. It is "" for other margin modes.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > notionalLever       | String    | Leverage of currency<br>Applicable to `Futures mode`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > stgyEq              | String    | Strategy equity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > isoUpl              | String    | Isolated unrealized profit and loss of currency<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > spotInUseAmt        | String    | Spot in use amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > clSpotInUseAmt      | String | User-defined spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > maxSpotInUse        | String | Max possible spot risk offset amount<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > spotIsoBal          | String    | Spot isolated balance<br>Applicable to copy trading<br>Applicable to `Spot mode`/`Futures mode`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > smtSyncEq           | String     | Smart sync equity<br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > spotCopyTradingEq   | String | Spot smart sync equity. <br>The default is "0", only applicable to copy trader.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > spotBal             | String | Spot balance. The unit is currency, e.g. BTC. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > openAvgPx           | String | Spot average cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > accAvgPx            | String | Spot accumulated cost price. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > spotUpl             | String | Spot unrealized profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > spotUplRatio        | String | Spot unrealized profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > totalPnl            | String | Spot accumulated profit and loss. The unit is USD. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > totalPnlRatio       | String | Spot accumulated profit and loss ratio. [More details](https://www.okx.com/help/i-introduction-of-spot)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > colRes               | String          | Platform level collateral restriction status<br> `0`: The restriction is not enabled.<br> `1`: The restriction is not enabled. But the crypto is close to the platform's collateral limit.<br> `2`: The restriction is enabled. This crypto can't be used as margin for your new orders. This may result in failed orders. But it will still be included in the account's adjusted equity and doesn't impact margin ratio.<br> Refer to [Introduction to the platform collateralized borrowing limit] (https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details. |
| > colBorrAutoConversion | String         | Risk indicator of auto conversion. Divided into multiple levels from 1-5, the larger the number, the more likely the repayment will be triggered. The default will be 0, indicating there is no risk currently. 5 means this user is undergoing auto conversion now, 4 means this user will undergo auto conversion soon whereas 1/2/3 indicates there is a risk for auto conversion.<br> Applicable to `Spot mode`/`Futures mode`/`Multi-currency margin`/`Portfolio margin`<br> When the total liability for each crypto set as collateral exceeds a certain percentage of the platform's total limit, the auto-conversion mechanism may be triggered. This may result in the automatic sale of excess collateral crypto if you've set this crypto as collateral and have large borrowings. To lower this risk, consider reducing your use of the crypto as collateral or reducing your liabilities.<br> Refer to [Introduction to the platform collateralized borrowing limit] (https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details.|
| > collateralRestrict | Boolean | ~~Platform level collateralized borrow restriction<br> `true`<br> `false`~~(deprecated, use colRes instead) |
| > collateralEnabled    | Boolean   | `true`: Collateral enabled<br>`false`: Collateral disabled<br>Applicable to `Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                   |
| > autoLendStatus | String | Auto lend status<br> `unsupported`: auto lend is not supported by this currency<br> `off`: auto lend is supported but turned off<br> `pending`: auto lend is turned on but pending matching<br> `active`: auto lend is turned on and matched |
| > autoLendMtAmt | String | Auto lend currency matched amount<br> Return "0" when autoLendStatus is `unsupported/off/pending`. Return matched amount when autoLendStatus is `active` |

<aside class="notice">
 "" will be returned for inapplicable fields with the current account level.
</aside>
