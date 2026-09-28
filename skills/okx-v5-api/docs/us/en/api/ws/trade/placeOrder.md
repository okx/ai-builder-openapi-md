---
site: us
lang: en
name: WS / Place order
type: guide
---

### WS / Place order

You can place an order only if you have sufficient funds.<br><br>

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 60 requests per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 4 requests per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

Rate limit of this endpoint will also be affected by the rules [Sub-account rate limit] (/docs-v5/en/#overview-rate-limits-sub-account-rate-limit) and [Fill ratio based sub-account rate limit] (/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit).

<aside class="notice">
Rate limit is shared with the `Place order` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1512",
  "op": "order",
  "args": [
    {
      "side": "buy",
      "instIdCode": 123456,
      "tdMode": "isolated",
      "ordType": "market",
      "sz": "100"
    }
  ]
}
```

#### Request Parameters

| Parameter      | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|:---------------| :------ | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String  | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                        |
| op             | String  | Yes      | Operation<br>`order`                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| args           | Array of objects   | Yes      | Request parameters                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > instIdCode   | Integer   | 是       | Instrument ID code.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > tdMode       | String  | Yes      | Trade mode <br>Margin mode `isolated` `cross`  <br>Non-Margin mode `cash`<br>`spot_isolated` (only applicable to SPOT lead trading, `tdMode` should be `spot_isolated` for `SPOT` lead trading.)   <br><br><font color="red">Event contracts symbols only support `isolated`</font>                                                                                                                                                                                                                                                                                                                    |
| > ccy          | String  | Conditional | Margin currency<br>Optional in general. Required for `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`.                                                                                                                                                                                                                                                                                                                                                  |
| > clOrdId      | String  | No       | Client Order ID as assigned by the client  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                           |
| > tag          | String  | No       | Order tag <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                                                                                                                                                                                                            |
| > side         | String  | Yes      | Order side, `buy` `sell`                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > posSide      | String  | Conditional | Position side  <br>The default is `net` in the `net` mode <br>It is required in the `long/short` mode, and can only be `long` or `short`. <br>Only applicable to `FUTURES`/`SWAP`.                                                                                                                                                                                                                                                                          |
| > ordType      | String  | Yes      | Order type <br>`market`: Market order, only applicable to `SPOT/MARGIN/FUTURES/SWAP` <br>`limit`: limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order  <br>`optimal_limit_ioc`: Market order with immediate-or-cancel order<br>`mmp`: Market Maker Protection (only applicable to Option in Portfolio Margin mode)<br>`mmp_and_post_only`: Market Maker Protection and Post-only order(only applicable to Option in Portfolio Margin mode)<br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.) |
| > sz           | String  | Yes      | Quantity to buy or sell.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > px           | String  | Conditional | Order price. Only applicable to `limit`,`post_only`,`fok`,`ioc`,`mmp`,`mmp_and_post_only` order.<br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                                     |
| > speedBump | String | Conditional | Speed bump<br>`1`: Event contract speed bumps (the delay duration will be changed subject to adjustment without prior notice). Required for non-post-only orders of `EVENTS` symbols. |
| > outcome | String | Conditional | The market outcome users trade on.<br>`yes`<br>`no`<br>Only applicable and required for `EVENTS` |
| > pxUsd        | String | Conditional | Place options orders in `USD` <br>Only applicable to options <br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                                                                          |
| > pxVol        | String | Conditional | Place options orders based on implied volatility, where 1 represents 100% <br>Only applicable to options <br>When placing an option order, one of px/pxUsd/pxVol must be filled in, and only one can be filled in                                                                                                                                                                                                                                            |
| > reduceOnly   | Boolean | No       | Whether the order can only reduce the position size.  <br>Valid options: `true` or `false`. The default value is `false`.<br>Only applicable to `MARGIN` orders, and `FUTURES`/`SWAP` orders in `net` mode <br>Only applicable to `Futures mode` and `Multi-currency margin`                                                                                                                                                                        |
| > tgtCcy       | String  | No       | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                          |
| > banAmend     | Boolean | No       | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: `true` or `false`. The default value is `false`.<br>If `true`, system will not amend and reject the market order if user does not have sufficient funds. <br>Only applicable to SPOT Market Orders                                                                                                                                                     |
| > pxAmendType  | String  | No       | The price amendment type for orders<br>`0`: Do not allow the system to amend to order price if `px` exceeds the price limit <br>`1`: Allow the system to amend the price to the best available value within the price limit if `px` exceeds the price limit<br> The default value is `0`         |
| > tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |
| > slippagePct   | String | No       | Maximum acceptable slippage for spot and spot margin market-side orders, where `tgtCcy` is the received currency (`base_ccy` for buy, `quote_ccy` for sell).<br>Range: `0` to `0.05` (0% to 5%, inclusive). Up to 2 decimal places of the percentage, e.g., `0.01` (1%) and `0.0123` (1.23%) are accepted; `0.01234` (1.234%) is rejected.<br>If not specified or empty, defaults to `0.00%`.<br>Slippage cannot be modified on an existing order. Cancel and resubmit to change the slippage setting.<br>Only applicable to `SPOT` and `SPOT margin` `market` orders. |
| > stpMode      | String | No          | Self trade prevention mode.  <br>`cancel_maker`,`cancel_taker`, `cancel_both`.<br>Cancel both does not support FOK <br><br>The account-level acctStpMode will be used to place orders. The default value of this field is `cancel_maker`. Users can log in to the webpage through the master account to modify this configuration. Users can also utilize the stpMode request parameter of the placing order endpoint to determine the stpMode of a certain order.|
| > rpiTakerAccess | Boolean | No | Default `false`.<br>When `true`, the order can access RPI liquidity, for `limit`, `market`, `fok`, and `ioc` orders.<br>A speedbump applies to all `ordType` including `post_only` when `rpiTakerAccess` is `true`.<br>Not inherited on amend — must be re-specified on each amend request (omitted = `false` for that amend).<br>`isElpTakerAccess` remains accepted as an alias until October 31, 2026. |
| > rpiPxRound | Boolean | No | Default `false`. Effective only on RPI maker orders (`ordType: rpi`). When `true`, a price that violates the RPI maker spacing rule is auto-rounded outward to the nearest placeable, non-crossing level instead of being rejected. Silently ignored on non-RPI orders and on `OPTION`/`EVENTS`. |
| expTime        | String | No     | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                      |

> Successful Response Example

```json
{
  "id": "1512",
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "ts":"1695190491421",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Failure Response Example

```json
{
  "id": "1512",
  "op": "order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "ts":"1695190491421",
      "sCode": "5XXXX",
      "sMsg": "not exist"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When Format Error

```json
{
  "id": "1512",
  "op": "order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

#### Response Parameters

| Parameter | Type   | Description                        |
| :-------- | :----- | :--------------------------------- |
| id        | String | Unique identifier of the message   |
| op        | String | Operation                          |
| code      | String | Error Code                         |
| msg       | String | Error message                      |
| data      | Array of objects  | Data                               |
| > ordId   | String | Order ID                           |
| > clOrdId | String | Client Order ID as assigned by the client       |
| > tag     | String | Order tag                          |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Rejection or success message of event execution.               |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
tdMode<br>
Trade Mode, when placing an order, you need to specify the trade mode.<br>
<strong>Spot mode:</strong><br>
- SPOT and OPTION buyer: cash<br>
<strong>Futures mode:</strong><br>
- Isolated MARGIN: isolated<br>
- Cross MARGIN: cross<br>
- SPOT: cash<br>
- Cross FUTURES/SWAP/OPTION: cross<br>
- Isolated FUTURES/SWAP/OPTION: isolated<br>
<strong>Multi-currency margin:</strong><br>
- Isolated MARGIN: isolated<br>
- Cross SPOT: cross<br>
- Cross FUTURES/SWAP/OPTION: cross<br>
- Isolated FUTURES/SWAP/OPTION: isolated<br>
<strong>Portfolio margin:</strong><br>
- Isolated MARGIN: isolated<br>
- Cross SPOT: cross<br>
- Cross FUTURES/SWAP/OPTION: cross<br>
- Isolated FUTURES/SWAP/OPTION: isolated
</aside>

<aside class="notice">
clOrdId<br>
clOrdId is a user-defined unique order identifier at the User ID level. If provided in the request parameters, it will be included in the response and can be used as a request parameter to query, cancel, and amend orders. <br>
clOrdId cannot duplicate any existing clOrdId of all current pending orders.
</aside>

<aside class="notice">
posSide<br>
Position side, this parameter is not mandatory in <strong>net</strong> mode. If you pass it through, the only valid value is <strong>net</strong>.<br>
In <strong>long/short</strong> mode, it is mandatory. Valid values are <strong>long</strong> or <strong>short</strong>.<br>
In <strong>long/short</strong> mode, <strong>side</strong> and <strong>posSide</strong> need to be specified in the combinations below:<br>
Open long: buy and open long (side: fill in buy; posSide: fill in long)<br>
Open short: sell and open short (side: fill in sell; posSide: fill in short)<br>
Close long: sell and close long (side: fill in sell; posSide: fill in long)<br>
Close short: buy and close short (side: fill in buy; posSide: fill in short)<br>
Portfolio margin mode: Expiry Futures and Perpetual Futures only support net mode
</aside>

<aside class="notice">
ordType <br>
Order type. When creating a new order, you must specify the order type. The order type you specify will affect: 1) what order parameters are required, and 2) how the matching system executes your order. The following are valid order types: <br>
limit: Limit order, which requires specified sz and px. <br>
market: Market order. For SPOT and MARGIN, market order will be filled with market price (by swiping opposite order book). For Expiry Futures and Perpetual Futures, market order will be placed to order book with most aggressive price allowed by Price Limit Mechanism. For OPTION, market order is not supported yet. As the filled price for market orders cannot be determined in advance, OKX reserves/freezes your quote currency by an additional 5% for risk check. <br>
post_only: Post-only order, which the order can only provide liquidity to the market and be a maker. If the order would have executed on placement, it will be canceled instead. <br>
fok: Fill or kill order. If the order cannot be fully filled, the order will be canceled. The order would not be partially filled. <br>
ioc: Immediate or cancel order. Immediately execute the transaction at the order price, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book. <br>
optimal_limit_ioc: Market order with ioc (immediate or cancel). Immediately execute the transaction of this market order, cancel the remaining unfilled quantity of the order, and the order quantity will not be displayed in the order book. Only applicable to Expiry Futures and Perpetual Futures.
</aside>

<aside class="notice">
sz<br>
Quantity to buy or sell. <br>
For SPOT/MARGIN Buy and Sell Limit Orders, it refers to the quantity in base currency. <br>
For MARGIN Buy Market Orders, it refers to the quantity in quote currency. <br>
For MARGIN Sell Market Orders, it refers to the quantity in base currency. <br>
For SPOT Market Orders, it is set by tgtCcy. <br>
For FUTURES/SWAP/OPTION orders, it refers to the number of contracts.
</aside>

<aside class="notice">
reduceOnly<br>
When placing an order with this parameter set to true, it means that the order will reduce the size of the position only<br>
For the same MARGIN instrument, the coin quantity of all reverse direction pending orders adds `sz` of new `reduceOnly` order cannot exceed the position assets. After the debt is paid off, if there is a remaining size of orders, the position will not be opened in reverse, but will be traded in SPOT.<br>
For the same FUTURES/SWAP instrument, the sum of the current order size and all reverse direction reduce-only pending orders which's price-time priority is higher than the current order, cannot exceed the contract quantity of position.<br>
Only applicable to `Futures mode` and `Multi-currency margin`<br>
Only applicable to `MARGIN` orders, and `FUTURES`/`SWAP` orders in `net` mode<br>
Notice: Under long/short mode of Expiry Futures and Perpetual Futures, all closing orders apply the reduce-only feature which is not affected by this parameter.

</aside>

<aside class="notice">
tgtCcy<br>
This parameter is used to specify the order quantity in the order request is denominated in the quantity of base or quote currency. This is applicable to SPOT Market Orders only.<br>
Base currency: base_ccy<br>
Quote currency: quote_ccy
<br>
If you use the Base Currency quantity for buy market orders or the Quote Currency for sell market orders, please note:
<br>
1. If the quantity you enter is greater than what you can buy or sell, the system will execute the order according to your maximum buyable or sellable quantity. If you want to trade according to the specified quantity, you should use Limit orders.
<br>
2. When the market price is too volatile, the locked balance may not be sufficient to buy the Base Currency quantity or sell to receive the Quote Currency that you specified. We will change the quantity of the order to execute the order based on best effort principle based on your account balance. In addition, we will try to over lock a fraction of your balance to avoid changing the order quantity.
<br>
2.1 Example of base currency buy market order:
<br>
Taking the market order to buy 10 LTCs as an example, and the user can buy 11 LTC. At this time, if 10 < 11, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 3,000 USDT, as 200*10 < 3,000, the market order of 10 LTC is fully executed;
If the market is too volatile and the LTC-USDT market price becomes 400, 400*10 > 3,000, the user's locked balance is not sufficient to buy using the specified amount of base currency, the user's maximum locked balance of 3,000 USDT will be used to settle the trade. Final transaction quantity becomes 3,000/400 = 7.5 LTC.
<br>
2.2 Example of quote currency sell market order:
<br>
Taking the market order to sell 1,000 USDT as an example, and the user can sell 1,200 USDT, 1,000 < 1,200, the order is accepted. When the LTC-USDT market price is 200, and the locked balance of the user is 6 LTC, as 1,000/200 < 6, the market order of 1,000 USDT is fully executed;
If the market is too volatile and the LTC-USDT market price becomes 100, 100*6 < 1,000, the user's locked balance is not sufficient to sell using the specified amount of quote currency, the user's maximum locked balance of 6 LTC will be used to settle the trade. Final transaction quantity becomes 6 * 100 = 600 USDT.
</aside>

<aside class="notice">
px<br>
The value for px must be a multiple of tickSz for OPTION orders.<br>
If not, the system will apply the rounding rules below. Using tickSz 0.0005 as an example:<br>
The px will be rounded up to the nearest 0.0005 when the remainder of px to 0.0005 is more than 0.00025 or `px` is less than 0.0005.<br>
The px will be rounded down to the nearest 0.0005 when the remainder of px to 0.0005 is less than 0.00025 and `px` is more than 0.0005.
</aside>

<aside class="notice">
Mandatory self trade prevention (STP)<br>
The trading platform imposes mandatory self trade prevention at master account level, which means the accounts under the same master account, including master account itself and all its affiliated sub-accounts, will be prevented from self trade. The account-level acctStpMode will be used to place orders by default. The default value of this field is `cancel_maker`. Users can log in to the webpage through the master account to modify this configuration. Users can also utilize the stpMode request parameter of the placing order endpoint to determine the stpMode of a certain order.<br>
Mandatory self trade prevention will not lead to latency. <br>
There are three STP modes. The STP mode is always taken based on the configuration in the taker order.<br>
1. Cancel Maker: This is the default STP mode, which cancels the maker order to prevent self-trading. Then, the taker order continues to match with the next order based on the order book priority.<br>
2. Cancel Taker: The taker order is canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled and then canceled. FOK orders are always honored and canceled if they would result in self-trading.<br>
3. Cancel Both: Both taker and maker orders are canceled to prevent self-trading. If the user's own maker order is lower in the order book priority, the taker order is partially filled. Then, the remaining quantity of the taker order and the first maker order are canceled. FOK orders are not supported in this mode.
</aside>
