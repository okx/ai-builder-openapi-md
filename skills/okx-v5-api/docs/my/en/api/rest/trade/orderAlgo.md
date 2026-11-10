---
site: my
lang: en
name: POST / Place algo order
method: POST
path: /api/v5/trade/order-algo
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### POST / Place algo order

The algo order includes `trigger` order, `oco` order, `chase` order, `conditional` order, `twap` order and trailing order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 1 request per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

#### HTTP Request

`POST /api/v5/trade/order-algo`

> Request Example

```shell
# Place Take Profit / Stop Loss Order
POST /api/v5/trade/order-algo
body
{
    "instId":"BTC-USDT",
    "tdMode":"cross",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}

# Place Trigger Order
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"side": "buy",
	"tdMode": "cross",
	"posSide": "net",
	"sz": "1",
	"ordType": "trigger",
	"triggerPx": "25920",
	"triggerPxType": "last",
	"orderPx": "-1",
	"attachAlgoOrds": [{
		"attachAlgoClOrdId": "",
		"slTriggerPx": "100",
		"slOrdPx": "600",
		"tpTriggerPx": "25921",
		"tpOrdPx": "2001"
	}]
}

# Place Trailing Stop Order
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"tdMode": "cross",
	"side": "buy",
	"ordType": "move_order_stop",
	"sz": "10",
	"posSide": "net",
	"callbackRatio": "0.05",
	"reduceOnly": true
}

# Place TWAP Order
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"tdMode": "cross",
	"side": "buy",
	"ordType": "twap",
	"sz": "10",
	"posSide": "net",
	"szLimit": "10",
	"pxLimit": "100",
	"timeInterval": "10",
	"pxSpread": "10"
}

# Iceberg order
POST /api/v5/trade/order-algo
body
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "smart_iceberg",
    "sz": "100",
    "szLimit": "10",
    "lmtOrderNumber": "5",
    "aggressiveness": "conservative",
    "pxLimit": "95000",
    "triggerParams": [
        {
            "triggerAction": "start",
            "triggerStrategy": "price",
            "triggerPx": "90000",
            "triggerCond": "cross_down"
        }
    ]
}

```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# One-way stop order
result = tradeAPI.place_algo_order(
    instId="BTC-USDT",
    tdMode="cross",
    side="buy",
    ordType="conditional",
    sz="2",
    tpTriggerPx="15",
    tpOrdPx="18"
)
print(result)
```

#### Request Parameters

| Parameter     | Type    | Required        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|:--------------|:--------|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String  | Yes             | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| tdMode        | String  | Yes             | Trade mode<br>Margin mode `cross` `isolated`<br>Non-Margin mode `cash`<br>`spot_isolated` (only applicable to SPOT lead trading)<br>Note: `isolated` is not available in multi-currency margin mode and portfolio margin mode.                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ccy           | String  | No              | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| side          | String  | Yes             | Order side, `buy` `sell`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| posSide       | String  | Conditional     | Position side <br>Required in `long/short` mode and only be `long` or `short`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ordType       | String  | Yes             | Order type  <br>`conditional`: One-way stop order<br>`oco`: One-cancels-the-other order<br>`chase`: chase order, only applicable to FUTURES and SWAP<br>`trigger`: Trigger order<br>`move_order_stop`: Trailing order<br>`twap`: TWAP order<br>`smart_iceberg`: Iceberg order                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| sz            | String  | Conditional     | Quantity to buy or sell<br>Either `sz` or `closeFraction` is required.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| tag           | String  | No              | Order tag  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| tgtCcy        | String  | No              | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` traded with Market buy `conditional` order<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| algoClOrdId   | String | No              | Client-supplied Algo ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| closeFraction | String | Conditional     | Fraction of position to be closed when the algo order is triggered. <br>Currently the system supports fully closing the position only so the only accepted value is `1`. For the same position, only one TPSL pending order for fully closing the position is supported. <br>This is only applicable to `FUTURES` or `SWAP` instruments.<br>If `posSide` is `net`, `reduceOnly` must be `true`.<br>This is only applicable if `ordType` is `conditional` or `oco`.<br>This is only applicable if the stop loss and take profit order is executed as market order.<br>This is not supported in Portfolio Margin mode.<br>Either `sz` or `closeFraction` is required. |
| tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |

<b>Take Profit / Stop Loss Order</b>

<aside class="notice">
Predefine the price you want the order to trigger a market order to execute immediately or it will place a limit order. <br>This type of order will not freeze your free margin in advance.
</aside>

learn more about [Take Profit / Stop Loss Order](/help/11015447687437)

| Parameter       | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------|:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tpTriggerPx     | String | No       | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| tpTriggerPxType | String | No       | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| tpOrdPx         | String | No       | Take-profit order price <br>For condition TP order, if you fill in this parameter, you should fill in the take-profit trigger price as well.<br>For limit TP order, you need to fill in this parameter, but the take-profit trigger price doesn’t need to be filled. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| tpOrdKind       | String | No       | TP order kind<br>`condition`<br>`limit`<br> The default is `condition` |
| slTriggerPx     | String | No       | Stop-loss trigger price <br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| slTriggerPxType | String | No       | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| slOrdPx         | String | No       | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| cxlOnClosePos   | Boolean | No       | Whether the TP/SL order placed by the user is associated with the corresponding position of the instrument. If it is associated, the TP/SL order will be canceled when the position is fully closed; if it is not, the TP/SL order will not be affected when the position is fully closed. <br>Valid values: <br>`true`: Place a TP/SL order associated with the position <br>`false`: Place a TP/SL order that is not associated with the position <br>The default value is `false`. If `true` is passed in, users must pass reduceOnly = true as well, indicating that when placing a TP/SL order associated with a position, it must be a reduceOnly order. <br>Only applicable to `Futures mode` and `Multi-currency margin`. |
| reduceOnly      | Boolean | No       | Whether the order can only reduce the position size. <br>Valid options: `true` or `false`. The default value is `false`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

<aside class="notice">
Take Profit / Stop Loss Order<br>
When placing net TP/SL order (ordType=conditional) and both take-profit and stop-loss parameters are sent, only stop-loss logic will be performed and take-profit logic will be ignored.
</aside>

<b>Chase order</b>
<aside class="notice">
It will place a Post Only order immediately and amend it continuously<br> 
Chase order and corresponding Post Only order can't be amended.
</aside>

| Parameter           | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                  |
|:--------------------| :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| chaseType           | String | No       | Chase type.<br>`distance`: distance from best bid/ask price, the default value.<br>`ratio`: ratio.                                                                                                                                                                                                                                                         |
| chaseVal             | String | No      | Chase value.<br>It represents distance from best bid/ask price when `chaseType` is distance. <br>For USDT-margined contract, the unit is USDT. <br>For USDC-margined contract, the unit is USDC. <br>For Crypto-margined contract, the unit is USD. <br>It represents ratio when `chaseType` is ratio. 0.1 represents 10%.<br> The default value is 0. |
| maxChaseType       | String | Conditional       | Maximum chase type.<br>`distance`: maximum distance from best bid/ask price<br>`ratio`: the ratio. <br><br> maxChaseTyep and maxChaseVal need to be used together or none of them.                                                                                                                                                                             |
| maxChaseVal        | String | Conditional       | Maximum chase value.<br>It represents maximum distance when `maxChaseType` is distance.<br>It represents ratio when `maxChaseType` is ratio. 0.1 represents 10%.                                                                                                                                                                                           |
| reduceOnly      | Boolean | No       | Whether the order can only reduce the position size. <br>Valid options: `true` or `false`. The default value is `false`.                                                                                                                                                                                                                                     |

<b>Trigger Order</b>

<aside class="notice">
Use a trigger order to place a market or limit order when a specific price level is crossed. <br>When a Trigger Order is triggered, if your account balance is lower than the order amount, the system will automatically place the order based on your current balance. <br>Trigger orders do not freeze assets when placed.<br> Only applicable to SPOT/FUTURES/SWAP  
</aside>

learn more about [Trigger Order](/help/11015447687437)

| Parameter           | Type   | Required | Description                                                                                                                                                                                                                                                                 |
|:--------------------| :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| triggerPx           | String | Yes       | The price level that activates this algo order. Unit: same as `px` for the instrument. Which price feed is compared depends on `triggerPxType` (default: last trade price). Direction: for a sell stop-loss, trigger must be below orderPx; for a buy stop, above orderPx. Error codes 51046–51049 are returned for direction violations.                                                                                                                                                                                                                                                                               |
| orderPx             | String | Conditional       | Price of the order submitted when `triggerPx` is reached. This is separate from `triggerPx` (which determines when the algo activates). Set to `-1` to submit a market order when triggered; set to a specific price to submit a limit order. Not applicable when `advanceOrdType` is `chase` (a chase has no fixed price).                                                                                                                                                                                                      |
| advanceOrdType      | String | No       | Sub-order type for trigger orders.<br>`fok`: Fill-or-kill order<br>`ioc`: Immediate-or-cancel order<br>`chase`: Chase limit order. Only applicable to FUTURES and SWAP.<br>Default is "", limit or market (controlled by `orderPx`).                                                                                                                                                                 |
| advChaseParams      | Array of objects | Conditional | Chase parameters. Required when `advanceOrdType` is `chase`. |
| > chaseType         | String | Conditional | Chase distance unit.<br>`distance` (default): absolute price distance from the best bid/ask, in settlement currency.<br>`ratio`: percentage. |
| > chaseVal          | String | Conditional | Chase value. When `chaseType` is `distance`, the distance from the best bid/ask in settlement currency; when `ratio`, `0.1` = 10%.<br>Default `0` tracks the best bid/ask directly; a value `>0` sets a distance. |
| > maxChaseType      | String | Conditional | Maximum chase distance unit. `distance` or `ratio`. Must pair with `maxChaseVal`. |
| > maxChaseVal       | String | Conditional | Maximum chase distance value. Positive. Must pair with `maxChaseType`. The chase auto-cancels when its deviation reaches this value. |
| triggerPxType       | String | No   | Trigger price type:<br>`last`: triggers when any trade occurs at or beyond `triggerPx` — most responsive but vulnerable to brief price wicks in thin markets.<br>`index`: triggers on the underlying multi-exchange composite index — stable, not affected by OKX-specific wicks.<br>`mark`: triggers on OKX mark price — smoothed and wick-resistant; recommended for derivatives.<br>`last` is the only option available for SPOT instruments. The default is `last`.                                                                                                                                                     |
| attachAlgoOrds      | Array of objects | No     | Attached SL/TP orders info<br>Applicable to `Futures mode/Multi-currency margin/Portfolio margin`<br>Not applicable when `advanceOrdType` is `chase`.                                                                                                                                                                 |
| > attachAlgoClOrdId | String          | No     | Client-supplied Algo ID when placing order attaching TP/SL.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing TP/SL order once the general order is filled completely. |
| > tpTriggerPx       | String | No       | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                        |
| > tpTriggerRatio    | String | No       | Take profit trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP. <br> Only one of `tpTriggerPx` and `tpTriggerRatio` can be passed <br> If the main order is a buy order, it must be greater than 0, and if the main order is a sell order, it must be between -1 and 0.                                                                                                                                                                                                            |
| > tpTriggerPxType   | String | No       | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price<br>The default is `last`                                                                                                                                             |
| > tpOrdPx           | String | No       | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                            |
| > slTriggerPx       | String | No       | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                     |
| > slTriggerRatio    | String | No       | Stop-loss trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP. <br> Only one of `slTriggerPx` and `slTriggerRatio` can be passed <br> If the main order is a buy order, it must be between 0 and 1, and if the main order is a sell order, it must be greater than 0.                                                                                                                                                                                                            |
| > slTriggerPxType   | String | No       | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                 |
| > slOrdPx           | String | No       | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                         |
| > callbackRatio     | String | Conditional | Callback ratio, e.g. `0.05` represents 5%.<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > callbackSpread    | String | Conditional | Callback spread (price distance).<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > activePx          | String | No          | Activation price.<br>The trailing stop is activated when the market price reaches the activation price. After activation, the system starts calculating the actual trigger price. If not provided, the trailing stop is activated immediately upon order placement.<br>Only applicable when `ordType` = `move_order_stop` |

<b>Trailing Stop Order</b>

<aside class="notice">
A trailing stop order is a stop order that tracks the market price. Its trigger price changes with the market price. Once the trigger price is reached, a market order is placed.<br>
Actual trigger price for sell orders and short positions = Highest price after order placement – Trail variance (Var.), or Highest price after placement × (1 – Trail variance) (Ratio).<br>
Actual trigger price for buy orders and long positions = Lowest price after order placement + Trail variance, or Lowest price after order placement × (1 + Trail variance).<br>
You can use the activation price to set the activation condition for a trailing stop order.
</aside>

learn more about [Trailing Stop Order](/help/11015447687437)

| Parameter         | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
|:------------------| :----- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| callbackRatio     | String | Conditional  | Callback price ratio, e.g. `0.01` represents `1%`<br>Either `callbackRatio` or `callbackSpread` is allowed to be passed.                                                                                                                                                                                                                                                                                                             |
| callbackSpread    | String | Conditional  | Callback price variance                                                                                                                                                                                                                                                                                                                                                              |
| activePx          | String | No       | Active price<br>The system will only start tracking the market and calculating your trigger price after the activation price is reached. If you don’t set a price, your order will be activated as soon as it’s placed.                                                                                                                                                                                                              |
| reduceOnly        | Boolean | No       | Whether the order can only reduce the position size. <br>Valid options: `true` or `false`. The default value is `false`.<br>This parameter is only valid in the `FUTRUES`/`SWAP` net mode, and is ignored in the long/short mode.                                                                                                                                                                                                    |

<b>TWAP Order</b>

<aside class="notice">
Time-weighted average price (TWAP) strategy splits your order and places smaller orders at regular time intervals.<br>
It is a strategy that will attempt to execute an order which trades in slices of order quantity at regular intervals of time as specified by users.<br>
</aside>

learn more about [TWAP Order](/help/xiii-time-weighted-average-price-twap)

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                  |
| :-------- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| pxVar    | String | Conditional  | Price variance by percentage, range between [0.0001 ~ 0.01], e.g. `0.01` represents `1%`<br>Take buy orders as an example. When the market price is lower than the limit price, small buy orders will be placed above the best bid price within a certain range. This parameter determines the range by percentage.<br>Either `pxVar` or `pxSpread` is allowed to be passed. |
| pxSpread | String | Conditional  | Price variance by constant, should be no less then 0 (no upper limit)<br>Take buy orders as an example. When the market price is lower than the limit price, small buy orders will be placed above the best bid price within a certain range. This parameter determines the range by constant.                                                                               |
| szLimit  | String | Yes       | Average amount<br>Take buy orders as an example. When the market price is lower than the limit price, a certain amount of buy orders will be placed above the best bid price within a certain range. This parameter determines the amount.                                                                                                                                   |
| pxLimit  | String | Yes       | Price Limit, should be no less then 0 (no upper limit)<br>Take buy orders as an example. When the market price is lower than the limit price, small buy orders will be placed above the best bid price within a certain range. This parameter represents the limit price.                                                                                                    |
| timeInterval    | String | Yes  | Time interval in unit of `second`<br>ake buy orders as an example. When the market price is lower than the limit price, small buy orders will be placed above the best bid price within a certain range based on the time cycle. This parameter represents the time cycle.                                                                                                   |

<b>Iceberg Order</b>

| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| szLimit | String | Yes | Minimum order size per execution. Only applicable to `smart_iceberg` |
| lmtOrderNumber | String | Yes | Number of limit order splits. Only applicable to `smart_iceberg` |
| aggressiveness | String | Yes | Aggressiveness level. Only applicable to `smart_iceberg`<br>`radical`: Faster fill<br>`mid`: Faster fill with better price<br>`conservative`: Queue at best bid/ask |
| pxLimit | String | No | Price limit. Only applicable to `smart_iceberg` |
| triggerParams | Array of objects | No | Trigger parameters. If the list is empty, the order is triggered immediately by default. Only applicable to `smart_iceberg` |
| > triggerAction | String | Yes | Trigger action<br>`start`: Start iceberg order |
| > triggerStrategy | String | Yes | Trigger strategy<br>`instant`: Trigger immediately<br>`price`: Trigger by price<br>`rsi`: Trigger by RSI indicator<br>Default is `instant` |
| > triggerPx | String | No | Trigger price<br>Only valid when `triggerStrategy` is `price` |
| > triggerCond | String | No | Trigger condition<br>`cross_up` / `cross_down` / `above` / `below` / `cross`<br>Only valid when `triggerStrategy` is `rsi` |
| > timeframe | String | No | K-line type: 3m / 5m / 15m / 30m / 1H / 4H / 1D<br>Only valid when `triggerStrategy` is `rsi` |
| > thold | String | No | Threshold, range [1, 100]<br>Only valid when `triggerStrategy` is `rsi` |
| > timePeriod | String | No | RSI calculation period. Default and fixed value is `14`. Only valid when `triggerStrategy` is `rsi` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "order1234",
            "algoId": "1836487817828872192",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------|:---------|:-----------------------------------------------------------|
| algoId        | String   | Algo ID                                                    |
| clOrdId       | String   | <del>Client Order ID as assigned by the client</del>(Deprecated)     |
| algoClOrdId     | String   | Client-supplied Algo ID                                    |
| sCode         | String   | The code of the event execution result, `0` means success. |
| sMsg          | String   | Rejection message if the request is unsuccessful.          |
| tag           | String   | Order tag                                                                                                                                                                                                                                                                   |
