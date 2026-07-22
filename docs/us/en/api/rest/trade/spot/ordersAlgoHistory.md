---
site: us
lang: en
name: GET / Algo order history
method: GET
path: /api/v5/trade/orders-algo-history
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / Algo order history

Retrieve a list of all algo orders under the current account in the last 3 months.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/orders-algo-history`

> Request Example

```shell
GET /api/v5/trade/orders-algo-history?ordType=conditional&state=effective
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve a list of all one-way stop algo orders
result = tradeAPI.order_algos_history(
    state="effective",
    ordType="conditional"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                                                              |
|:----------|:-------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| ordType   | String | Yes         | Order type<br>`conditional`: One-way stop order<br>`oco`: One-cancels-the-other order <br>`trigger`: Trigger order <br>`move_order_stop`: Trailing order |
| state     | String | Conditional | State<br>`effective`<br>`canceled`<br>`order_failed`<br>Either `state` or `algoId` is required                                                           |
| algoId    | String | Conditional | Algo ID <br>Either `state` or `algoId` is required.                                                                                                      |
| instType  | String | No          | Instrument type<br>`SPOT`                                                                                             |
| instId    | String | No          | Instrument ID, e.g. `BTC-USDT`                                                                                                                           |
| after     | String | No          | Pagination of data to return records earlier than the requested `algoId`                                                                                 |
| before    | String | No          | Pagination of data to return records new than the requested `algoId`                                                                                     |
| limit     | String | No          | Number of results per request. The maximum is `100`. The default is `100`                                                                                |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "buy",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "681096944655273984",
            "amendPxOnTriggerType": "",
            "attachAlgoOrds": [],
            "cTime": "1708658165774",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "last": "51014.6",
            "lever": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "-1",
            "ordType": "trigger",
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
            "state": "canceled",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "timeInterval": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "triggerPx": "100",
            "triggerPxType": "last",
            "triggerTime": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**        | **Type**        | **Description**                                                                                                                                                                                                                                                             |
|:---------------------|:----------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType             | String          | Instrument type                                                                                                                                                                                                                                                             |
| instId               | String          | Instrument ID                                                                                                                                                                                                                                                               |
| ccy                  | String          | Margin currency<br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`.                                                                                                                                                                                  |
| ordId                | String          | Latest order ID                                                                                                                                                                                                                                                             |
| ordIdList            | Array of strings          | Order ID list. There will be multiple order IDs when there is TP/SL splitting order.                                                                                                                                                                                        |
| algoId               | String          | Algo ID                                                                                                                                                                                                                                                                     |
| clOrdId              | String          | Client Order ID as assigned by the client                                                                                                                                                                                                                                   |
| sz                   | String          | Quantity to buy or sell                                                                                                                                                                                                                                                     |
| closeFraction        | String          | Fraction of position to be closed when the algo order is triggered                                                                                                                                                                                                          |
| ordType              | String          | Order type                                                                                                                                                                                                                                                                  |
| side                 | String          | Order side                                                                                                                                                                                                                                                                  |
| posSide              | String          | Position side                                                                                                                                                                                                                                                               |
| tdMode               | String          | Trade mode                                                                                                                                                                                                                                                                  |
| tgtCcy               | String          | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                          |
| state                | String          | State<br>`effective` <br>`canceled` <br>`order_failed` <br>`partially_failed`                                                                                                                                                                                               |
| lever                | String          | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                                                                |     |
| tpTriggerPx          | String          | Take-profit trigger price.                                                                                                                                                                                                                                                  |
| tpTriggerPxType      | String          | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                        |
| tpOrdPx              | String          | Take-profit order price.                                                                                                                                                                                                                                                    |
| slTriggerPx          | String          | Stop-loss trigger price.                                                                                                                                                                                                                                                    |
| slTriggerPxType      | String          | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                          |
| slOrdPx              | String          | Stop-loss order price.                                                                                                                                                                                                                                                      |
| triggerPx            | String          | trigger price.                                                                                                                                                                                                                                                              |
| triggerPxType        | String          | trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                    |
| ordPx                | String          | Order price for the trigger order                                                                                                                                                                                                                                                                 |
| actualSz             | String          | Actual order quantity                                                                                                                                                                                                                                                       |
| actualPx             | String          | Actual order price                                                                                                                                                                                                                                                          |
| tag                  | String          | Order tag                                                                                                                                                                                                                                                                   |
| actualSide           | String          | Actual trigger side, `tp`: take profit `sl`: stop loss<br>Only applicable to oco order and conditional order                                                                                                                                                                                                                      |
| triggerTime          | String          | Trigger time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                   |
| pxVar                | String          | Price ratio <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                          |
| pxSpread             | String          | Price variance <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                       |
| szLimit              | String          | Average amount <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                       |
| pxLimit              | String          | Price Limit <br>Only applicable to `iceberg` order or `twap` order                                                                                                                                                                                                          |
| timeInterval         | String          | Time interval <br>Only applicable to `twap` order                                                                                                                                                                                                                           |
| callbackRatio        | String          | Callback price ratio<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                          |
| callbackSpread       | String          | Callback price variance<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                       |
| activePx             | String          | Active price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                                  |
| moveTriggerPx        | String          | Trigger price<br>Only applicable to `move_order_stop` order                                                                                                                                                                                                                 |
| reduceOnly           | String          | Whether the order can only reduce the position size. Valid options: true or false.                                                                                                                                                                                          |
| quickMgnType         | String          | ~~Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>`manual`, `auto_borrow`, `auto_repay`~~  (Deprecated)                                                                                                                                       |
| last                 | String          | Last filled price while placing                                                                                                                                                                                                                                             |
| failCode             | String          | It represents that the reason that algo order fails to trigger. It is "" when the state is `effective`/`canceled`. There will be value when the state is `order_failed`, e.g. 51008;<br>Only applicable to Stop Order, Trailing Stop Order, Trigger order.                  |
| algoClOrdId          | String          | Client Algo Order ID as assigned by the client.                                                                                                                                                                                                                             |
| amendPxOnTriggerType | String          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                              |
| attachAlgoOrds       | Array of objects | Attached TP/SL or trailing stop order info<br>Applicable to `Futures mode`/`Multi-currency margin`/`Portfolio margin`                                                                                                                                                                 |
| > attachAlgoClOrdId  | String          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop.<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to algoClOrdId when placing the attached algo order once the general order is filled completely. |
| > tpTriggerPx        | String          | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                         |
| > tpTriggerPxType    | String          | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price<br>The default is `last`                                                                                                                                             |
| > tpOrdPx            | String          | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                             |
| > slTriggerPx        | String          | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                     |
| > slTriggerPxType    | String          | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                  |
| > slOrdPx            | String          | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                          |
| cTime                | String          | Creation time Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                   |
