---
site: us
lang: en
name: Get instruments
method: GET
path: /api/v5/account/instruments
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get instruments

Retrieve available instruments info of current account.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument Type

#### HTTP Request

`GET /api/v5/account/instruments`

> Request Example

```shell
GET /api/v5/account/instruments?instType=SPOT
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1" # Production trading: 0, Demo trading: 1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.get_instruments(instType="SPOT")
print(result)
```

#### Request Parameters

| Parameter      | Type   | Required    | Description                                                                                                               |
|:---------------| :----- |:------------|:--------------------------------------------------------------------------------------------------------------------------|
| instType       | String | Yes         | Instrument type<br>`SPOT`: Spot<br>`MARGIN`: Margin<br>`SWAP`: Perpetual Futures<br>`FUTURES`: Expiry Futures<br>`OPTION`: Option<br>`EVENTS`: Event Contracts |
| seriesId       | String | Conditional | Series ID, e.g. `BTC-ABOVE-DAILY`. Required when instType is `EVENTS` |
| instFamily     | String | Conditional | Instrument family<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`. If instType is `OPTION`, `instFamily` is required.                                    |
| instId         | String | No          | Instrument ID                                                                                                             |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "elp": "0",
            "rpi": "0",
            "expTime": "",
            "futureSettlement": false,
            "groupId": "4",
            "instFamily": "",
            "instId": "BTC-EUR",
            "instType": "SPOT",
            "lever": "",
            "listTime": "1704876947000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "1000000",
            "maxPlatOILmt": "1000000000",
            "maxPlatOICoinLmt": "",
            "maxStopSz": "1000000",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "preMktSwTime": "",
            "posLmtPct": "30",
            "posLmtAmt": "2500000",
            "quoteCcy": "EUR",
            "tradeQuoteCcyList": [
                "EUR"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "1",
            "uly": "",
            "instIdCode": 1000000000,   
            "instCategory": "1",
            "initPxLmtPct": "0.05",
            "floatPxLmtPct": "0.03",
            "maxPxLmtPct": "0.15",
            "upcChg": [
                {
                    "param": "tickSz",
                    "newValue": "0.0001",
                    "effTime": "1704876947000"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**    | **Type** | **Description**                                                                                                                                                                                                                                                            |
|:-----------------| :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType         | String | Instrument type                                                                                                                                                                                                                                                            |
| seriesId         | String | Series ID, e.g. `BTC-ABOVE-DAILY`. Only applicable to `EVENTS`                                                                                                                                                                                                             |
| instId           | String | Instrument ID,  e.g. `BTC-USD-SWAP`                                                                                                                                                                                                                                        |
| uly              | String | Underlying, e.g. `BTC-USD` <br>Only applicable to `MARGIN/FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                         |
| groupId | String | Instrument trading fee group ID<br>Spot:<br>`3`: Spot TRY<br>`5`: Spot BRL<br>`7`: Spot AED<br>`8`: Spot AUD<br>`10`: Spot SGD<br>`11`: Spot zero<br>`12`: Spot group one<br>`13`: Spot group two<br>`14`: Spot group three<br>`15`: Spot special rule<br>`17`: Spot stablecoin<br>`22`: Spot RWA group two<br><br>Expiry futures:<br>`5`: Expiry futures group one<br>`6`: Expiry futures group two<br>`8`: XPERP group two<br>`10`: XPERP RWA group two<br><br>Perpetual futures:<br>`4`: Perpetual futures group one<br>`5`: Perpetual futures group two<br>`6`: SWAP RWA group one<br>`7`: SWAP RWA group two<br><br>Options:<br>`1`: Options crypto-margined<br><br>**instType and groupId should be used together to determine a trading fee group. Users should use this endpoint together with [fee rates endpoint](/docs-v5/en/#trading-account-rest-api-get-fee-rates) to get the trading fee of a specific symbol.** <br><br>**Some enum values may not apply to you; the actual return values shall prevail.**|
| instFamily       | String | Instrument family, e.g. `BTC-USD` <br>Only applicable to `MARGIN/FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                  |
| baseCcy          | String | Base currency, e.g. `BTC`  in`BTC-USDT` <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                             |
| quoteCcy         | String | Quote currency, e.g. `USDT` in `BTC-USDT`   <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                         |
| settleCcy        | String | Settlement and margin currency, e.g. `BTC`  <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                               |
| ctVal            | String | Contract value   <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                          |
| ctMult           | String | Contract multiplier   <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                     |
| ctValCcy         | String | Contract value currency  <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                  |
| optType          | String | Option type, `C`: Call  `P`: put <br>Only applicable to `OPTION`                                                                                                                                                                                                           |
| stk              | String | Strike price  <br>Only applicable to `OPTION`                                                                                                                                                                                                                              |
| listTime         | String | Listing time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                  |
| auctionEndTime   | String | ~~The end time of call auction, Unix timestamp format in milliseconds, e.g. `1597026383085` <br>Only applicable to `SPOT` that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime)~~                                                 |
| contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. `1597026383085`.<br> Only applicable to `SPOT`/`MARGIN` that are listed through call auction or prequote, return "" in other cases.|
| preMktSwTime | String	 | The time a pre-market instrument switched to normal trading, Unix timestamp format in milliseconds, e.g. `1597026383085`. <br> Only applicable to pre-market `SWAP` and pre-market X-Perp `FUTURES`. Populated when a pre-market X-Perp converts to a normal X-Perp |
| openType | String | Open type<br> `fix_price`: fix price opening<br>`pre_quote`: pre-quote<br>`call_auction`: call auction <br>Only applicable to `SPOT`/`MARGIN`, return "" for all other business lines |
| elp | String | ELP maker permission<br>`0`: ELP is not enabled for this symbol<br>`1`: ELP is enabled for this symbol, but current users don't have permission to place ELP orders for it. <br>`2`: ELP is enabled for this symbol, and current users have permission to place ELP orders for it. <br><br>It doesn't mean there will be ELP liquidity when elp is `1/2`. |
| rpi | String | RPI maker permission.<br>`0`: not enabled for this instrument<br>`1`: enabled, but the current user has no permission to place RPI orders<br>`2`: enabled and permitted<br>A `1`/`2` value does not imply RPI liquidity is present.<br>`elp` remains accepted as an alias until October 31, 2026. |
| expTime          | String | Expiry time <br>Applicable to `SPOT`/`MARGIN`/`FUTURES`/`SWAP`/`OPTION`. For `FUTURES`/`OPTION`, it is natural delivery/exercise time. It is the instrument offline time when there is `SPOT/MARGIN/FUTURES/SWAP/` manual offline. Update once change.                     |
| lever            | String | Max Leverage,   <br>Not applicable to `SPOT`, `OPTION`                                                                                                                                                                                                                     |
| tickSz           | String | Tick size, e.g. `0.0001`.<br>For `OPTION`/`EVENTS`, it is the minimum tickSz among tick band. Use "Get instrument tick bands" endpoint with the corresponding `instType` for accurate tickSz per price range.                                                                                                                        |
| lotSz            | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                                                              |
| minSz            | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                                                    |
| ctType           | String | Contract type<br>`linear`: linear contract<br>`inverse`: inverse contract <br>Only applicable to `FUTURES`/`SWAP`                                                                                                                                                          |
| state            | String | Instrument status<br>`live` <br>`suspend`<br>`rebase`: can't be traded during rebasing<br>`post_only`: only post-only orders are accepted; existing post-only orders can be amended and cancelled. Other order types (market, IOC, FOK, normal limit) are rejected.<br>`preopen` e.g. Futures and options contracts rollover from generation to trading start; certain symbols before they go live<br>`test`: Test pairs, can't be traded<br>`settling`: Settling, only applicable to `EVENTS`                                                            |
| ruleType         | String | Trading rule types<br>`normal`: normal trading<br>`pre_market`: pre-market trading, including pre-market X-Perp `FUTURES`<br>`rebase_contract`: pre-market rebase contract<br>`xperp`: perpetual-style futures, only applicable to certain `FUTURES` contracts. A pre-market X-Perp changes from `pre_market` to `xperp` after it converts to a normal X-Perp |
| posLmtAmt        | String | Maximum position value (USD) for this instrument at the user level (shared across master and sub-accounts), based on the notional value of all same-direction open positions and resting orders. The effective user limit is max(posLmtAmt, oiUSD × posLmtPct). Applicable to `SWAP`/`FUTURES`.                                                         |
| posLmtPct        | String | Maximum position ratio (e.g., 30 for 30%) a user (shared across master and sub-accounts) may hold relative to the platform's current total position value. The effective user limit is max(posLmtAmt, oiUSD × posLmtPct). Applicable to `SWAP`/`FUTURES`.                                                     |
| maxPlatOILmt     | String | Platform-wide maximum position value (USD) for this instrument. If the platform total open interest (USD) reaches or exceeds this value, all users’ new opening orders for this instrument are rejected; otherwise, orders pass.<br>Applicable to `SWAP`/`FUTURES`                               |
| maxPlatOICoinLmt | String | Platform-wide maximum position value (coins) for this instrument. If the platform total open interest (coins) reaches or exceeds this value, all users’ new opening orders for this instrument are rejected; otherwise, orders pass.<br>Applicable to `SWAP`/`FUTURES`                               |
| longPosRemainingQuota | String | The remaining long position value (USD) the user is permitted to open, netting all existing long positions and resting buy orders. The quota is shared across the master account and all subaccounts. |
| shortPosRemainingQuota| String | The remaining short position value (USD) the user is permitted to open, netting all existing short positions and resting sell orders. The quota is shared across the master account and all subaccounts. |
| maxLmtSz         | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                   |
| maxMktSz         | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                           |
| maxLmtAmt        | String | Max USD amount for a single limit order                                                                                                                                                                                                                                    |
| maxMktAmt        | String | Max USD amount for a single market order <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                            |
| maxTwapSz        | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`. <br/> The minimum order quantity of a single TWAP order is minSz*2 |
| maxIcebergSz     | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                 |
| maxTriggerSz     | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                 |
| maxStopSz        | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                      |
| futureSettlement | Boolean | Whether daily settlement for expiry feature is enabled<br>Applicable to `FUTURES` `cross`                                                                                                                                                                                  |
| tradeQuoteCcyList | Array of strings | List of quote currencies available for trading, e.g. ["USD", "USDC"].|
| instIdCode | Integer | Instrument ID code. <br>For simple binary encoding, you must use `instIdCode` instead of `instId`.<br>For the same `instId`, it's value may be different between production and demo trading. <br> It is `null` when the value is not generated. |
| instCategory | String | The asset category of the instrument’s base asset (the first segment of the instrument ID). For example, for `BTC-USDT-SWAP`, the `instCategory` represents the asset category of `BTC`. <br>`1`: Crypto <br>`3`: Stocks <br>`4`: Commodities <br>`5`: Forex <br>`6`: Bonds <br>`""`: Not available |
| initPxLmtPct | String | Initial price-limit band applied during the first 10 minutes after contract listing, e.g. `0.05` represents 5%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| floatPxLmtPct | String | Floating price-limit band during normal trading, e.g. `0.03` represents 3%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| maxPxLmtPct | String | Maximum price-limit cap (hard ceiling on order-price deviation from the index price), e.g. `0.15` represents 15%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| upcChg | Array of objects | Upcoming changes. It is [] when there is no upcoming change.|
| > param | String | The parameter name to be updated. <br> `tickSz`<br> `minSz`: For `FUTURES`/`SWAP`, `lotSz` will be modified synchronously.<br> `maxMktSz`|
| > newValue | String | The parameter value that will replace the current one.|
| > effTime | String | Effective time. Unix timestamp format in milliseconds, e.g. `1597026383085`|

<aside class="notice">
listTime and contTdSwTime<br>
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".
</aside>

<aside class="notice">
state<br>
For `SPOT`, `MARGIN`, `SWAP`, and `FUTURES`, the state changes from `preopen` to `live` when the `listTime` is reached. For `OPTION` contracts, the state may change to `live` slightly after `listTime` due to internal processing. It is recommended to verify that `state` is `live` before placing orders.<br>
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.
</aside>
