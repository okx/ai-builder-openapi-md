---
site: my
lang: en
name: GET / Algo order details
method: GET
path: /api/v5/trade/order-algo
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / Algo order details

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/order-algo`

> Request Example

```shell
GET /api/v5/trade/order-algo?algoId=1753184812254216192
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                            |
|:----------|:-------|:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId  | String | Conditional    | Algo ID<br>Either `algoId` or `algoClOrdId` is required.If both are passed, `algoId` will be used.                                                                                                                          |
| algoClOrdId     | String | Conditional | Client-supplied Algo ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                         |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "1753184812254216192",
            "amendPxOnTriggerType": "0",
            "attachAlgoOrds": [],
            "cTime": "1724751378980",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "chaseType": "",
            "chaseVal": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "0",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTradeBorrowMode": "",
            "last": "62916.5",
            "lever": "",
            "linkedOrd": {
                "ordId": ""
            },
            "maxChaseType": "",
            "maxChaseVal": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "",
            "ordType": "conditional",
            "posSide": "net",
            "pxLimit": "",
            "pxSpread": "",
            "pxVar": "",
            "quickMgnType": "",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "state": "live",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "timeInterval": "",
            "tpOrdPx": "-1",
            "tpTriggerPx": "10000",
            "tpTriggerPxType": "last",
            "triggerPx": "",
            "triggerPxType": "",
            "triggerTime": "",
            "tradeQuoteCcy": "USDT",
            "uTime": "1724751378980"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**                | **Type** | **Description**                                                                                                                                                                                                                                                             |
|:-----------------------------| :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType                     | String   | Instrument type                                                                                                                                                                                                                                                             |
| instId                       | String   | Instrument ID                                                                                                                                                                                                                                                               |
| ccy                    | String          | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`, `FUTURES` and `SWAP` contracts.                                                                                                                                                                                                                                                   |
| ordId                        | String   | Latest order ID. It will be deprecated soon                                                                                                                                                                                                                                                           |
| ordIdList                    | Array of strings   | Order ID list. There will be multiple order IDs when there is TP/SL splitting order. Empty for a trigger+chase (the spawned order is an algo — see `subAlgoIdList`).                                                                                                                                                                                        |
| subAlgoIdList                | Array of strings   | `algoId`(s) of the algo order(s) spawned when the trigger fires. For `advanceOrdType` `chase`, holds the spawned chase order's `algoId` after the trigger fires; empty before then. Mirrors `ordIdList`, which records spawned regular orders. |
| algoId                       | String   | Algo ID                                                                                                                                                                                                                                                                     |
| clOrdId                      | String   | Client Order ID as assigned by the client                                                                                                                                                                                                                                   |
| sz                           | String   | Quantity to buy or sell                                                                                                                                                                                                                                                     |
| closeFraction                | String   | Fraction of position to be closed when the algo order is triggered                                                                                                                                                                                                          |
| ordType                      | String   | Order type                                                                                                                                                                                                                                                                  |
| side                         | String   | Order side                                                                                                                                                                                                                                                                  |
| posSide                      | String   | Position side                                                                                                                                                                                                                                                               |
| tdMode                       | String   | Trade mode                                                                                                                                                                                                                                                                  |
| tgtCcy                       | String   | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                          |
| state                        | String   | State <br>`live` <br>`pause` <br>`partially_effective`<br>`effective` <br>`canceled` <br>`order_failed`<br>`partially_failed`                                                                                                                                               |
| lever                        | String   | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP` |
| tpTriggerPx                  | String   | Take-profit trigger price.                                                                                                                                                                                                                                                  |
| tpTriggerPxType              | String   | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                        |
| tpOrdPx                      | String   | Take-profit order price.                                                                                                                                                                                                                                                    |
| slTriggerPx                  | String   | Stop-loss trigger price.                                                                                                                                                                                                                                                    |
| slTriggerPxType              | String   | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                          |
| slOrdPx                      | String   | Stop-loss order price.                                                                                                                                                                                                                                                      |
| triggerPx                    | String   | trigger price.                                                                                                                                                                                                                                                              |
| triggerPxType                | String   | trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                    |
| ordPx                        | String   | Order price for the trigger order                                                                                                                                                                                                                                                                 |
| advanceOrdType               | String        | Sub-order type for trigger orders.<br>`fok`: Fill-or-kill order<br>`ioc`: Immediate-or-cancel order<br>`chase`: Chase limit order<br>Default is "". |
| advChaseParams               | Array of objects | Chase parameters. Present when `advanceOrdType` is `chase`. |
| > chaseType                  | String | Chase distance unit. `distance` or `ratio`. |
| > chaseVal                   | String | Chase value. `0` tracks the best bid/ask directly; `>0` is a distance. |
| > maxChaseType               | String | Maximum chase distance unit. `distance` or `ratio`. |
| > maxChaseVal                | String | Maximum chase distance value. |
| actualSz                     | String   | Actual order quantity                                                                                                                                                                                                                                                       |
| actualPx                     | String   | Actual order price                                                                                                                                                                                                                                                          |
| tag                          | String   | Order tag                                                                                                                                                                                                                                                                   |
| actualSide                   | String   | Actual trigger side, `tp`: take profit `sl`: stop loss<br>Only applicable to oco order and conditional order                                                                                                                                                               |
| triggerTime                  | String   | Trigger time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                   |
| pxVar                        | String | Price ratio <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                          |
| pxSpread                     | String | Price variance <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                       |
| szLimit                      | String | Average amount <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                       |
| pxLimit                      | String | Price Limit <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                          |
| timeInterval                 | String | Time interval <br>Only applicable to `twap` order                                                                                                                                                                                                                           |
| callbackRatio                | String   | Callback price ratio<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                          |
| callbackSpread               | String  | Callback price variance<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                       |
| activePx                     | String   | Active price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                                  |
| moveTriggerPx                | String | Trigger price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                                 |
| reduceOnly                   | String   | Whether the order can only reduce the position size. Valid options: true or false.                                                                                                                                                                                          |
| quickMgnType                 | String   | ~~Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>`manual`, `auto_borrow`, `auto_repay`~~  (Deprecated)                                                                                                                                       |
| last                         | String | Last filled price while placing                                                                                                                                                                                                                                             |
| failCode                     | String | It represents that the reason that algo order fails to trigger. It is "" when the state is `effective`/`canceled`. There will be value when the state is `order_failed`, e.g. 51008;<br>Only applicable to Stop Order, Trailing Stop Order, Trigger order.                  |
| algoClOrdId                  | String | Client-supplied Algo ID                                                                                                                                                                                                                                                     |
| amendPxOnTriggerType         | String            | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                              |
| attachAlgoOrds               | Array of objects | Attached TP/SL or trailing stop order info<br>Applicable to `Futures mode/Multi-currency margin/Portfolio margin`                                                                                                                                                                 |
| > attachAlgoClOrdId          | String          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing the attached algo order once the general order is filled completely. |
| > tpTriggerPx                | String | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                         |
| > tpTriggerRatio             | String | Take profit trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP.                                                                                                                                                                                                               |
| > tpTriggerPxType            | String | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                          |
| > tpOrdPx                    | String | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                             |
| > slTriggerPx                | String | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                     |
| > slTriggerRatio             | String | Stop-loss trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP.                                                                                                                                                                                                               |
| > slTriggerPxType            | String | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                            |
| > slOrdPx                    | String | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                          |
| > callbackRatio       | String | Callback ratio, e.g. `0.05` represents 5% |
| > callbackSpread      | String | Callback spread (price distance) |
| > activePx            | String | Activation price |
| linkedOrd                    | Object | Linked TP order detail, only applicable to SL order that comes from the one-cancels-the-other (OCO) order that contains the TP limit order.                                                                                                                                                                               |
| > ordId                      | String | Order ID                                                                                                                                                                                                                                                                    |
| cTime                        | String   | Creation time Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                   |
| uTime                        | String            | Order updated time, Unix timestamp format in milliseconds, e.g. 1597026383085                                                                                                                                                                                               |
| isTradeBorrowMode            | String            | Whether borrowing currency automatically<br> true<br> false<br>Only applicable to `trigger order`, `trailing order` and `twap order`                                                                                   |
| chaseType                    | String | Chase type. Only applicable to `chase` order. |
| chaseVal                     | String | Chase value. Only applicable to `chase` order.                                 |
| maxChaseType                 | String | Maximum chase type. Only applicable to `chase` order.                          |
| maxChaseVal                  | String | Maximum chase value. Only applicable to `chase` order.                         |
| tradeQuoteCcy | String | The quote currency used for trading.|
