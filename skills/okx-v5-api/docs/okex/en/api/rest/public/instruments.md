---
site: okex
lang: en
name: Get instruments
method: GET
path: /api/v5/public/instruments
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get instruments

Retrieve a list of instruments with open contracts for OKX. Retrieve available instruments info of current account, please refer to [Get instruments](/docs-v5/en/#trading-account-rest-api-get-instruments).

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP + Instrument Type

#### HTTP Request

`GET /api/v5/public/instruments`

> Request Example

```shell
GET /api/v5/public/instruments?instType=SPOT
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve a list of instruments with open contracts
result = publicDataAPI.get_instruments(
    instType="SPOT"
)
print(result)
```

#### Request Parameters

| Parameter      | Type   | Required | Description                                                                                                               |
|:---------------| :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------|
| instType       | String | Yes       | Instrument type<br>`SPOT`: Spot<br>`MARGIN`: Margin<br>`SWAP`: Perpetual Futures<br>`FUTURES`: Expiry Futures<br>`OPTION`: Option<br>`EVENTS`: Event Contracts |
| seriesId       | String | Conditional | Series ID, e.g. `BTC-ABOVE-DAILY`. Required when instType is `EVENTS` |
| instFamily     | String | Conditional  | Instrument family<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`. If instType is `OPTION`, `instFamily` is required.                                    |
| instId         | String | No        | Instrument ID                                                                                                             |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
      {
            "alias": "",
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "category": "1",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "expTime": "",
            "futureSettlement": false,
            "groupId": "1",
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "10",
            "listTime": "1606468572000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "",
            "maxStopSz": "",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "preMktSwTime": "",
            "quoteCcy": "USDT",
            "tradeQuoteCcyList": [
                "USDT"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "0.1",
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
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                                                                                            |
|:--------------| :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType      | String | Instrument type                                                                                                                                                                                                                                                            |
| seriesId      | String | Series ID, e.g. `BTC-ABOVE-DAILY`. Only applicable to `EVENTS`                                                                                                                                                                                                             |
| instId        | String | Instrument ID,  e.g. `BTC-USD-SWAP`                                                                                                                                                                                                                                        |
| uly           | String | Underlying, e.g. `BTC-USD` <br>Only applicable to `MARGIN/FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                         |
| groupId | String | Instrument trading fee group ID<br>Spot:<br>`3`: Spot TRY<br>`5`: Spot BRL<br>`7`: Spot AED<br>`8`: Spot AUD<br>`10`: Spot SGD<br>`11`: Spot zero<br>`12`: Spot group one<br>`13`: Spot group two<br>`14`: Spot group three<br>`15`: Spot special rule<br>`17`: Spot stablecoin<br>`22`: Spot RWA group two<br><br>Expiry futures:<br>`5`: Expiry futures group one<br>`6`: Expiry futures group two<br>`8`: XPERP group two<br>`10`: XPERP RWA group two<br><br>Perpetual futures:<br>`4`: Perpetual futures group one<br>`5`: Perpetual futures group two<br>`6`: SWAP RWA group one<br>`7`: SWAP RWA group two<br><br>Options:<br>`1`: Options crypto-margined<br><br>**instType and groupId should be used together to determine a trading fee group. Users should use this endpoint together with [fee rates endpoint](/docs-v5/en/#trading-account-rest-api-get-fee-rates) to get the trading fee of a specific symbol.** <br><br>**Some enum values may not apply to you; the actual return values shall prevail.**|
| instFamily    | String | Instrument family, e.g. `BTC-USD` <br>Only applicable to `MARGIN/FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                  |
| category      | String | Currency category. Note: this parameter is already deprecated                                                                                                                                                                                                              |
| baseCcy       | String | Base currency, e.g. `BTC`  in`BTC-USDT` <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                             |
| quoteCcy      | String | Quote currency, e.g. `USDT` in `BTC-USDT`   <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                         |
| settleCcy     | String | Settlement and margin currency, e.g. `BTC`  <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                               |
| ctVal         | String | Face value of one contract. Denomination depends on `ctType`: for linear contracts, `ctVal` is in the base currency (e.g., ctVal=0.01 BTC for BTC-USDT-SWAP); for inverse contracts, `ctVal` is in USD (e.g., ctVal=100 USD for BTC-USD-SWAP). Notional: linear = sz × ctVal × markPx (in quote ccy); inverse = sz × ctVal (in USD, fixed).   <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                          |
| ctMult        | String | Contract multiplier   <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                     |
| ctValCcy      | String | Contract value currency  <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                  |
| optType       | String | Option type, `C`: Call  `P`: put <br>Only applicable to `OPTION`                                                                                                                                                                                                           |
| stk           | String | Strike price  <br>Only applicable to `OPTION`                                                                                                                                                                                                                              |
| listTime      | String | Listing time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                  |
| auctionEndTime   | String | ~~The end time of call auction, Unix timestamp format in milliseconds, e.g. `1597026383085` <br>Only applicable to `SPOT` that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime)~~                                                 |
| contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. `1597026383085`.<br> Only applicable to `SPOT`/`MARGIN` that are listed through call auction or prequote, return "" in other cases.|
| preMktSwTime | String	 | The time a pre-market instrument switched to normal trading, Unix timestamp format in milliseconds, e.g. `1597026383085`. <br> Only applicable to pre-market `SWAP` and pre-market X-Perp `FUTURES`. Populated when a pre-market X-Perp converts to a normal X-Perp |
| openType | String | Open type<br> `fix_price`: fix price opening<br>`pre_quote`: pre-quote<br>`call_auction`: call auction <br>Only applicable to `SPOT`/`MARGIN`, return "" for all other business lines |
| expTime       | String | Expiry time <br>Applicable to `SPOT`/`MARGIN`/`FUTURES`/`SWAP`/`OPTION`. For `FUTURES`/`OPTION`, it is natural delivery/exercise time. It is the instrument offline time when there is `SPOT/MARGIN/FUTURES/SWAP/` manual offline. Update once change.                     |
| lever         | String | Exchange-defined maximum leverage ceiling for this instrument. The actual leverage available to a specific account may be lower based on VIP tier and position size. Use GET /api/v5/account/leverage-info for the user's current configured leverage. <br>Not applicable to `SPOT`, `OPTION`                                                                                                                                                                                                                                                                                                                     |
| tickSz        | String | Minimum price increment, e.g. `0.0001`.<br>For `OPTION`/`EVENTS`, it is the minimum tickSz among tick band. Use "Get instrument tick bands" endpoint with the corresponding `instType` for accurate tickSz per price range.                                                                                                                        |
| lotSz         | String | Lot size — the minimum order size increment. All order quantities (sz) must be a multiple of `lotSz`. Violation returns error 51121.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                                                              |
| minSz         | String | Minimum order size. Order size must satisfy both: sz ≥ `minSz` AND sz is a multiple of `lotSz`.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                                                    |
| ctType        | String | Contract type<br>`linear`: linear contract — margin, P&L, and settlement in the quote currency (e.g., USDT for BTC-USDT-SWAP).<br>`inverse`: inverse contract — margin, P&L, and settlement in the base currency (e.g., BTC for BTC-USD-SWAP). For inverse contracts, P&L in USD terms is non-linear: the USD value of a fixed BTC gain changes with the BTC price. <br>Only applicable to `FUTURES`/`SWAP`                                                                                                                                                          |
| alias         | String | Contract alias (deprecated — use expTime to obtain the delivery time, will be removed by the end of April 2026)<br>`this_week`<br>`next_week`<br>`this_month`<br>`next_month`<br>`quarter`<br>`next_quarter`<br>`third_quarter`<br>`this_five_years`: current 5-year contract<br>`next_five_years`: next 5-year contract<br>Only applicable to `FUTURES` |
| state         | String | Instrument status<br>`live` <br>`suspend`<br>`rebase`: can’t be traded during rebasing<br>`post_only`: only post-only orders are accepted; existing post-only orders can be amended and cancelled. Other order types (market, IOC, FOK, normal limit) are rejected.<br>`preopen`. e.g. There will be `preopen` before the Futures and Options new contracts state is live.<br>`test`: Test pairs, can’t be traded<br>`settling`: Settling, only applicable to `EVENTS`                                                                                    |
| ruleType | String | Trading rule types<br>`normal`: normal trading<br>`pre_market`: pre-market trading, including pre-market X-Perp `FUTURES`<br>`rebase_contract`: pre-market rebase contract<br>`xperp`: perpetual-style futures, only applicable to certain `FUTURES` contracts. A pre-market X-Perp changes from `pre_market` to `xperp` after it converts to a normal X-Perp |
| maxLmtSz      | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                   |
| maxMktSz      | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                           |
| maxLmtAmt     | String | Max USD amount for a single limit order                                                                                                                                                                                                                                    |
| maxMktAmt     | String | Max USD amount for a single market order <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                            |
| maxTwapSz     | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`. <br/> The minimum order quantity of a single TWAP order is minSz*2 |
| maxIcebergSz  | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                 |
| maxTriggerSz  | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                 |
| maxStopSz     | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                      |
| futureSettlement | Boolean | Whether daily settlement for expiry feature is enabled<br>Applicable to `FUTURES` `cross`                                                                                                                                                                                  |
| tradeQuoteCcyList | Array of strings | List of quote currencies available for trading, e.g. ["USD", "USDC”].|
| instIdCode | Integer | Instrument ID code. <br>For simple binary encoding, you must use `instIdCode` instead of `instId`.<br>For the same `instId`, it's value may be different between production and demo trading. <br> It is `null` when the value is not generated. |
| instCategory | String | The asset category of the instrument’s base asset (the first segment of the instrument ID). For example, for `BTC-USDT-SWAP`, the `instCategory` represents the asset category of `BTC`. <br>`1`: Crypto <br>`3`: Stocks <br>`4`: Commodities <br>`5`: Forex <br>`6`: Bonds <br>`""`: Not available |
| initPxLmtPct | String | Initial price-limit band applied during the first 10 minutes after contract listing, e.g. `0.05` represents 5%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| floatPxLmtPct | String | Floating price-limit band during normal trading, e.g. `0.03` represents 3%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| maxPxLmtPct | String | Maximum price-limit cap (hard ceiling on order-price deviation from the index price), e.g. `0.15` represents 15%. Use GET /api/v5/public/price-limit for the computed price limits.<br>Only applicable to `SPOT`/`MARGIN`/`SWAP`/`FUTURES`, returns `""` for `OPTION` and `EVENTS`. |
| rpiMinLevel | String | Minimum spacing between the RPI bid and RPI ask, in organic price levels. |
| rpiMinPxBand | String | Minimum distance from the opposite-side organic best price, in basis points (bps). |
| upcChg | Array of objects | Upcoming changes. It is [] when there is no upcoming change.|
| > param | String | The parameter name to be updated. <br> `tickSz`<br> `minSz`: For `FUTURES`/`SWAP`, `lotSz` will be modified synchronously.<br> `maxMktSz`|
| > newValue | String | The parameter value that will replace the current one.|
| > effTime | String | Effective time. Unix timestamp format in milliseconds, e.g. `1597026383085`|

<aside class="notice">
 When a new contract is going to be listed, the instrument data of the new contract will be available with status preopen.
 When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available <br>
</aside>

<aside class="notice">
 listTime and contTdSwTime<br>
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".
</aside>

<aside class="notice">
state<br>
For `SPOT`, `MARGIN`, `SWAP`, and `FUTURES`, the state changes from `preopen` to `live` when the `listTime` is reached. For `OPTION` contracts, the state may change to `live` slightly after `listTime` due to internal processing. It is recommended to verify that `state` is `live` before placing orders.<br>
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.
</aside>

<aside class="notice">
Instruments REST endpoints and WebSocket channel will update `expTime` once the delisting announcement is published.<br>
Instruments REST endpoint and WebSocket channel will update `listTime` once the listing announcement is published:<br>

1. For `SPOT/MARGIN/SWAP`, this event is only applicable to `instType`, `instId`, `listTime`, `state`.<br>
2. For `FUTURES`, this event is only applicable to `instType`, `instFamily`, `listTime`, `state`.<br>
3. Other fields will be "" temporarily, but they will be updated at least 5 minutes in advance of the `listTime`, then the WebSocket subscription using related `instId`/`instFamily` can be available.<br>
</aside>
