---
site: my
lang: en
name: WebSocket
type: guide
---

## WebSocket
### Instruments channel

The triggering scenarios for incremental data are:<br>
1. When there is any change to the instrument’s state (such as delivery of FUTURES, exercise of OPTION, listing of new contracts / trading pairs, trading suspension, etc.)<br>
2. When the trading parameters change (tickSz,minSz,maxMktSz)<br>
3. When the expTime or listTime changes<br>

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "instruments",
      "instType": "SPOT"
    }
  ]
}
```
```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "instruments",
          "instType": "SPOT"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```
#### Request Parameters

| Parameter  | Type   | Required | Description                                                            |
| ---------- | ------ | -------- | ---------------------------------------------------------------------- |
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>                            |
| args       | Array of objects  | Yes      | List of subscribed channels                                            |
| > channel  | String | Yes      | Channel name<br>`instruments`                                            |
| > instType | String | Yes      | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |

> Successful Response Example

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "instruments",
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"instruments\", \"instType\" : \"FUTURES\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter  | Type   | Required | Description                                                             |
| :--------- | :----- | :------- | :---------------------------------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event      | String | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error`                                |
| arg        | Object | No       | Subscribed channel                                                      |
| > channel  | String | Yes      | Channel name                                                            |
| > instType | String | Yes      | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| code       | String | No       | Error code                                                              |
| msg        | String | No       | Error message                                                           |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
  "arg": {
    "channel": "instruments",
    "instType": "SPOT"
  },
  "data": [
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
        "settleCcy": "",
        "state": "live",
        "ruleType": "normal",
        "stk": "",
        "tickSz": "0.1",
        "uly": "",
        "instIdCode": 1000000000,
        "instCategory": "1",
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

#### Push data parameters

| Parameter          | Type   | Description                                                                                                                                                                                                                                                                  |
|:-------------------| :----- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                | Object | Subscribed channel                                                                                                                                                                                                                                                           |
| > channel          | String | Channel name                                                                                                                                                                                                                                                                 |
| > instType         | String | Instrument type                                                                                                                                                                                                                                                              |
| data               | Array of objects  | Subscribed data                                                                                                                                                                                                                                                              |
| > instType         | String | Instrument type                                                                                                                                                                                                                                                              |
| > seriesId         | String | Series ID, e.g. `BTC-ABOVE-DAILY`. Only applicable to `EVENTS`                                                                                                                                                                                                               |
| > instId           | String | Instrument ID, e.g. `BTC-UST`                                                                                                                                                                                                                                                |
| > uly              | String | Underlying, e.g. `BTC-USD` <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                      |
| > groupId | String | Instrument trading fee group ID<br>Spot:<br>`3`: Spot TRY<br>`5`: Spot BRL<br>`7`: Spot AED<br>`8`: Spot AUD<br>`10`: Spot SGD<br>`11`: Spot zero<br>`12`: Spot group one<br>`13`: Spot group two<br>`14`: Spot group three<br>`15`: Spot special rule<br>`17`: Spot stablecoin<br>`22`: Spot RWA group two<br><br>Expiry futures:<br>`5`: Expiry futures group one<br>`6`: Expiry futures group two<br>`8`: XPERP group two<br>`10`: XPERP RWA group two<br><br>Perpetual futures:<br>`4`: Perpetual futures group one<br>`5`: Perpetual futures group two<br>`6`: SWAP RWA group one<br>`7`: SWAP RWA group two<br><br>Options:<br>`1`: Options crypto-margined<br><br>**instType and groupId should be used together to determine a trading fee group. Users should use this endpoint together with [fee rates endpoint](/docs-v5/en/#trading-account-rest-api-get-fee-rates) to get the trading fee of a specific symbol.** <br><br>**Some enum values may not apply to you; the actual return values shall prevail.**|
| > instFamily       | String | Instrument family, e.g. `BTC-USD` <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                               |
| > category         | String | Currency category. Note: this parameter is already deprecated                                                                                                                                                                                                                |
| > baseCcy          | String | Base currency, e.g. `BTC` in `BTC-USDT` <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                                 |
| > quoteCcy         | String | Quote currency, e.g. `USDT` in `BTC-USDT` <br>Only applicable to `SPOT`/`MARGIN`                                                                                                                                                                                               |
| > settleCcy        | String | Settlement and margin currency, e.g. `BTC` <br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                      |
| > ctVal            | String | Contract value                                                                                                                                                                                                                                                               |
| > ctMult           | String | Contract multiplier                                                                                                                                                                                                                                                          |
| > ctValCcy         | String | Contract value currency                                                                                                                                                                                                                                                      |
| > optType          | String | Option type<br>`C`: Call<br>`P`: Put<br>Only applicable to `OPTION`                                                                                                                                                                                                             |
| > stk              | String | Strike price<br>Only applicable to `OPTION`                                                                                                                                                                                                                                  |
| > listTime         | String | Listing time<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                     |
| > auctionEndTime   | String | ~~The end time of call auction, Unix timestamp format in milliseconds, e.g. `1597026383085` <br>Only applicable to `SPOT` that are listed through call auctions, return "" in other cases (deprecated, use contTdSwTime)~~                                                 |
| > contTdSwTime | String | Continuous trading switch time. The switch time from call auction, prequote to continuous trading, Unix timestamp format in milliseconds. e.g. `1597026383085`.<br> Only applicable to `SPOT`/`MARGIN` that are listed through call auction or prequote, return "" in other cases.|
| > preMktSwTime | String	 | The time a pre-market instrument switched to normal trading, Unix timestamp format in milliseconds, e.g. `1597026383085`. <br> Only applicable to pre-market `SWAP` and pre-market X-Perp `FUTURES`. Populated when a pre-market X-Perp converts to a normal X-Perp |
| > openType | String | Open type<br> `fix_price`: fix price opening<br>`pre_quote`: pre-quote<br>`call_auction`: call auction <br>Only applicable to `SPOT`/`MARGIN`, return "" for all other business lines |
| > expTime          | String | Expiry time<br>Applicable to `SPOT`/`MARGIN`/`FUTURES`/`SWAP`/`OPTION`. For `FUTURES`/`OPTION`, it is the delivery/exercise time. It can also be the delisting time of the trading instrument. Update once change.                                                                     |
| > lever            | String | Max Leverage<br>Not applicable to `SPOT`/`OPTION`, used to distinguish between `MARGIN` and `SPOT`.                                                                                                                                                                          |
| > tickSz           | String | Tick size, e.g. `0.0001`.<br>For `OPTION`/`EVENTS`, it is the minimum tickSz among tick band.                                                                                                                                                                                                |
| > lotSz            | String | Lot size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`                                                                                                                 |
| > minSz            | String | Minimum order size<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`                                                                                                       |
| > ctType           | String | Contract type<br>`linear`: linear contract<br>`inverse`: inverse contract<br>Only applicable to `FUTURES`/`SWAP`                                                                                                                                                               |
| > alias            | String | Contract alias (deprecated — use expTime to obtain the delivery time, will be removed by the end of April 2026)<br>`this_week`<br>`next_week`<br>`this_month`<br>`next_month`<br>`quarter`<br>`next_quarter`<br>`this_five_years`: current 5-year contract<br>`next_five_years`: next 5-year contract<br>Only applicable to `FUTURES` |
| > state            | String | Instrument status<br>`live`<br>`suspend`<br>`expired`<br>`rebase`: can't be traded during rebasing<br>`post_only`: only post-only orders are accepted; existing post-only orders can be amended and cancelled. Other order types (market, IOC, FOK, normal limit) are rejected.<br>`preopen`. e.g. There will be `preopen` before the Futures and Options new contracts state is live.<br>`test`: Test pairs, can't be traded<br>`settling`: Settling, only applicable to `EVENTS`                                                                           |
| > ruleType         | String | Trading rule types<br>`normal`: normal trading<br>`pre_market`: pre-market trading, including pre-market X-Perp `FUTURES`<br>`rebase_contract`: pre-market rebase contract<br>`xperp`: perpetual-style futures, only applicable to certain `FUTURES` contracts. A pre-market X-Perp changes from `pre_market` to `xperp` after it converts to a normal X-Perp |
| > maxLmtSz         | String | The maximum order quantity of a single limit order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                     |
| > maxMktSz         | String | The maximum order quantity of a single market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                             |
| > maxTwapSz        | String | The maximum order quantity of a single TWAP order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                      |
| > maxIcebergSz     | String | The maximum order quantity of a single iceBerg order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                   |
| > maxTriggerSz     | String | The maximum order quantity of a single trigger order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `base currency`.                                                                   |
| > maxStopSz        | String | The maximum order quantity of a single stop market order.<br>If it is a derivatives contract, the value is the number of contracts.<br>If it is `SPOT`/`MARGIN`, the value is the quantity in `USDT`.                                                                        |
| > futureSettlement | Boolean | Whether daily settlement for expiry feature is enabled<br>Applicable to `FUTURES` `cross`                                                                                                                                                                                  |
| > instIdCode | Integer | Instrument ID code. <br>For simple binary encoding, you must use `instIdCode` instead of `instId`.<br>For the same `instId`, it's value may be different between production and demo trading. <br> It is `null` when the value is not generated. |
| > instCategory | String | The asset category of the instrument’s base asset (the first segment of the instrument ID). For example, for `BTC-USDT-SWAP`, the `instCategory` represents the asset category of `BTC`. <br>`1`: Crypto <br>`3`: Stocks <br>`4`: Commodities <br>`5`: Forex <br>`6`: Bonds <br>`""`: Not available |
| > upcChg | Array of objects | Upcoming changes. It is [] when there is no upcoming change.|
| >> param | String | The parameter name to be updated. <br> `tickSz`<br> `minSz`: For `FUTURES`/`SWAP`, `lotSz` will be modified synchronously.<br> `maxMktSz`|
| >> newValue | String | The parameter value that will replace the current one.|
| >> effTime | String | Effective time. Unix timestamp format in milliseconds, e.g. `1597026383085`|

<aside class="notice"> 
Instrument status will trigger pushing of incremental data from instruments channel.
When a new contract is going to be listed, the instrument data of the new contract will be available with status preopen.
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument status will be changed to expired.
</aside>

<aside class="notice">
listTime and contTdSwTime<br>
For spot symbols listed through a call auction or pre-open, listTime represents the start time of the auction or pre-open, and contTdSwTime indicates the end of the auction or pre-open and the start of continuous trading. For other scenarios, listTime will mark the beginning of continuous trading, and contTdSwTime will return an empty value "".
</aside>

<aside class="notice">
state<br>
For `SPOT`, `MARGIN`, `SWAP`, and `FUTURES`, the state changes from `preopen` to `live` when the `listTime` is reached. For `OPTION` contracts, the state may change to `live` slightly after `listTime` due to internal processing. It is recommended to verify that `state` is `live` before placing orders. Certain symbols will now have `state:preopen` before they go live. Before going live, the instruments channel will push data for pre-listing symbols with `state:preopen`. If the listing is cancelled, the channel will send full data excluding the cancelled symbol, without additional notification. When the symbol goes live (at or shortly after `listTime` for `OPTION`), the channel will push data with `state:live`. Users can also query the corresponding data via the REST endpoint.<br>
When a product is going to be delisted (e.g. when a FUTURES contract is settled or OPTION contract is exercised), the instrument will not be available.
</aside>
