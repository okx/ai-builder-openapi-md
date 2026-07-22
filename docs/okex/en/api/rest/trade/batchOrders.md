---
site: okex
lang: en
name: POST / Place multiple orders
method: POST
path: /api/v5/trade/batch-orders
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Place multiple orders

Place orders in batches. Maximum 20 orders can be placed per request. <br>
Request parameters should be passed in the form of an array. Orders will be placed in turn<br>

#### Rate Limit: 300 orders per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 4 orders per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

Rate limit of this endpoint will also be affected by the rules [Sub-account rate limit] (/docs-v5/en/#overview-rate-limits-sub-account-rate-limit) and [Fill ratio based sub-account rate limit] (/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit).

<aside class="notice">
Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Place order`.
</aside>

#### HTTP Request

`POST /api/v5/trade/batch-orders`

> Request Example

```shell
 batch place order for SPOT
 POST /api/v5/trade/batch-orders
 body
 [
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b15",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    },
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b16",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    }
]

```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Place multiple orders 
place_orders_without_clOrdId = [
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b15", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"},
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b16", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"}
]

result = tradeAPI.place_multiple_orders(place_orders_without_clOrdId)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| :--------- | :------ | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId     | String  | Yes      | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| tdMode     | String  | Yes      | Trade mode<br>Margin mode `cross` `isolated`<br>Non-Margin mode `cash`<br>`spot_isolated` (only applicable to SPOT lead trading, `tdMode` should be `spot_isolated` for `SPOT` lead trading.)<br>Note: `isolated` is not available in multi-currency margin mode and portfolio margin mode.    <br><br><font color="red">Event contracts symbols only support `isolated`</font>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ccy        | String  | No       | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| clOrdId    | String  | No       | Client Order ID as assigned by the client  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                                                                                  |
| tag        | String  | No       | Order tag <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                                                                                                                                                                                                                                                                   |
| side       | String  | Yes      | Order side `buy` `sell`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| posSide    | String  | Conditional | Position side  <br>The default is `net` in the `net` mode <br>It is required in the `long/short` mode, and can only be `long` or `short`. <br>Only applicable to `FUTURES`/`SWAP`.                                                                                                                                                                                                                                                                                                                                 |
| ordType    | String  | Yes      | Order type <br>`market`: Market order, only applicable to `SPOT/MARGIN/FUTURES/SWAP` <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order  <br>`optimal_limit_ioc`: Market order with immediate-or-cancel order (applicable only to Expiry Futures and Perpetual Futures).<br>`mmp`: Market Maker Protection (only applicable to Option in Portfolio Margin mode)<br>`mmp_and_post_only`: Market Maker Protection and Post-only order(only applicable to Option in Portfolio Margin mode) <br>`elp`: Enhanced Liquidity Program order|
| sz         | String  | Yes      | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| px              | String  | Conditional | Order price. Only applicable to `limit`,`post_only`,`fok`,`ioc`,`mmp`,`mmp_and_post_only` order.<br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                                                                                           |
| speedBump | String | Conditional | Speed bump<br>`1`: Event contract speed bumps (the delay duration will be changed subject to adjustment without prior notice). Required for non-post-only orders of `EVENTS` symbols. |
| outcome | String | Conditional | The market outcome users trade on.<br>`yes`<br>`no`<br>Only applicable and required for `EVENTS` |
| pxUsd | String | Conditional | Place options orders in `USD` <br>Only applicable to options <br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                                                                                                                              |
| pxVol | String | Conditional | Place options orders based on implied volatility, where 1 represents 100% <br>Only applicable to options <br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                                                                                  |
| reduceOnly | Boolean | No       | Whether the order can only reduce position size.  <br>Valid options: `true` or `false`. The default value is `false`.<br>Only applicable to `MARGIN` orders, and `FUTURES`/`SWAP` orders in `net` mode <br>Only applicable to `Futures mode` and `Multi-currency margin`                                                                                                                                                                                                                                 |
| tgtCcy     | String  | No       | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                                                 |
| banAmend   | Boolean | No       | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: `true` or `false`. The default value is `false`.<br>If `true`, system will not amend and reject the market order if user does not have sufficient funds. <br>Only applicable to SPOT Market Orders                                                                                                                                                                                                               |
| pxAmendType            | String  | No          | The price amendment type for orders<br>`0`: Do not allow the system to amend to order price if `px` exceeds the price limit <br>`1`: Allow the system to amend the price to the best available value within the price limit if `px` exceeds the price limit<br> The default value is `0`         |
| tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |
| slippagePct     | String | No       | Maximum acceptable slippage for spot and spot margin market-side orders, where `tgtCcy` is the received currency (`base_ccy` for buy, `quote_ccy` for sell).<br>Range: `0` to `0.05` (0% to 5%, inclusive). Up to 2 decimal places of the percentage, e.g., `0.01` (1%) and `0.0123` (1.23%) are accepted; `0.01234` (1.234%) is rejected.<br>If not specified or empty, defaults to `0.00%`.<br>Slippage cannot be modified on an existing order. Cancel and resubmit to change the slippage setting.<br>Only applicable to `SPOT` and `SPOT margin` `market` orders. |
| stpMode | String | No          | Self trade prevention mode. <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both does not support FOK. <br><br>The account-level acctStpMode will be used to place orders by default. The default value of this field is `cancel_maker`. Users can log in to the webpage through the master account to modify this configuration. Users can also utilize the stpMode request parameter of the placing order endpoint to determine the stpMode of a certain order. |
| isElpTakerAccess | Boolean | No | ELP taker access<br>`true`: the request can trade with ELP orders but a speed bump will be applied<br>`false`: the request cannot trade with ELP orders and no speed bump<br><br>The default value is `false` while `true` is only applicable to ioc orders. |
| attachAlgoOrds         | Array of objects   | No          | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > attachAlgoClOrdId    | String   | No          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                                                                                                                                                                                                                                       |
| > tpTriggerPx          | String | Conditional | Take-profit trigger price<br>For condition TP order, if you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                                                                                                                                                                                                                                       |
| > tpTriggerRatio       | String | Conditional | Take profit trigger ratio, 0.3 represents 30% <br> Only one of `tpTriggerPx` and `tpTriggerRatio` can be passed <br> Only applicable to FUTURES and SWAP.<br>If the main order is a buy order, it must be greater than 0, and if the main order is a sell order, it must be between -1 and 0.                                                                              |
| > tpOrdPx              | String | Conditional | Take-profit order price <br>For condition TP order, if you fill in this parameter, you should fill in the take-profit trigger price as well. <br>For limit TP order, you need to fill in this parameter, take-profit trigger needn't to be filled.<br>If the price is -1, take-profit will be executed at the market price.                                                                                                                                                                                       |
| > tpOrdKind | String | No       | TP order kind<br>`condition`<br>`limit`<br> The default is `condition`                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > slTriggerPx          | String | Conditional | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                            |
| > slTriggerRatio       | String | Conditional | Stop-loss trigger ratio, 0.3 represents 30% <br> Only one of `slTriggerPx` and `slTriggerRatio` can be passed <br> Only applicable to FUTURES and SWAP.<br>If the main order is a buy order, it should be between 0 and 1, and if the main order is a sell order, it must be greater than 0.                                                                              |
| > slOrdPx              | String | Conditional | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price.                                                                                                                                                                                                                                                                                                                                     |
| > tpTriggerPxType      | String | No          | Take-profit trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price <br>The default is last                                                                                                                                                                                                                                                                                                                                                                                       |
| > slTriggerPxType      | String | No          | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price <br>The default is last                                                                                                                                                                                                                                                                                                                                                                                         |
| > sz                   | String | Conditional | Size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > amendPxOnTriggerType | String            | No          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. Whether `slTriggerPx` will move to `avgPx` when the first TP order is triggered<br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                                                                                                                                                                                      |
| > callbackRatio       | String | Conditional | Callback ratio, e.g. `0.05` represents 5%.<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > callbackSpread      | String | Conditional | Callback spread (price distance).<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > activePx            | String | No          | Activation price.<br>The trailing stop is activated when the market price reaches the activation price. After activation, the system starts calculating the actual trigger price. If not provided, the trailing stop is activated immediately upon order placement.<br>Only applicable when `ordType` = `move_order_stop` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":"",
            "subCode": ""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "tag":"",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":"",
            "subCode": ""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**    |
| :------------ | :------- | :--------------------------------------------------------- |
| code            | String   | The result code, `0` means success |
| msg             | String   | The error message, empty if the code is 0 |
| data            | Array of objects | Array of objects contains the response results |
| > ordId         | String   | Order ID           |
| > clOrdId       | String   | Client Order ID as assigned by the client                  |
| > tag           | String   | Order tag          |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > sCode         | String   | The code of the event execution result, `0` means success. |
| > sMsg          | String   | Rejection or success message of event execution.          |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123` <br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">

In the `Portfolio Margin` account mode, either all orders are accepted by the system successfully, or all orders are rejected by the system. 
</aside>

<aside class="notice">
clOrdId<br>
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders. <br>
clOrdId must be unique among all pending orders and the current request.
</aside>

<aside class="notice">
Rate limit of orders tagged as isElpTakerAccess:true<br>
- 50 orders per 2 seconds per User ID per instrument ID.<br>
- This rate limit is shared in Place order/Place multiple orders endpoints in REST/WebSocket<br>
</aside>
