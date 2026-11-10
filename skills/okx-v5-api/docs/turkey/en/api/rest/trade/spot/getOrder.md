---
site: turkey
lang: en
name: GET / Order details
method: GET
path: /api/v5/trade/order
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Order details

Retrieve order details.

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`GET /api/v5/trade/order`

> Request Example

```shell
GET /api/v5/trade/order?ordId=1753197687182819328&instId=BTC-USDT

```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve order details by ordId
result = tradeAPI.get_order(
    instId="BTC-USDT",
    ordId="680800019749904384"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                             |
| :-------- | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------|
| instId    | String | Yes      | Instrument ID, e.g. `BTC-USDT`<br>Only applicable to live instruments                                                                   |
| ordId     | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used                                          |
| clOrdId   | String | Conditional | Client Order ID as assigned by the client<br>If the `clOrdId` is associated with multiple orders, only the latest one will be returned. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.00192834",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "51858",
            "cTime": "1708587373361",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "-0.00000192834",
            "feeCcy": "BTC",
            "fillPx": "51858",
            "fillSz": "0.00192834",
            "fillTime": "1708587373361",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "680800019749904384",
            "ordType": "market",
            "pnl": "0",
            "posSide": "net",
            "px": "",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "filled",
            "stpId": "",
            "stpMode": "",
            "sz": "100",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "744876980",
            "uTime": "1708587373362"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**          | **Type**        | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|:-----------------------|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType               | String          | Instrument type<br>`SPOT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| instId                 | String          | Instrument ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| tgtCcy                 | String          | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency<br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                          |
| ccy                    | String          | Margin currency<br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`..                                                                                                                                                                                                                                                                                                                                                                                                |
| ordId                  | String          | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| clOrdId                | String          | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| tag                    | String          | Order tag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| px                     | String          | Price<br>For options, use coin as unit (e.g. BTC, ETH)                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| pxUsd                  | String          | Options price in USD. Only applicable to options; return "" for other instrument types                                                                                                                                                                                                                                                                                                                                                                                                    |
| pxVol                  | String          | Implied volatility of the options order. Only applicable to options; return "" for other instrument types                                                                                                                                                                                                                                                                                                                                                                                 |
| pxType                 | String          | Price type of options <br>`px`: Place an order based on price, in the unit of coin (the unit for the request parameter px is BTC or ETH) <br>`pxVol`: Place an order based on pxVol <br>`pxUsd`: Place an order based on pxUsd, in the unit of USD (the unit for the request parameter px is USD)                                                                                                                                                                                         |
| sz                     | String          | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| pnl                    | String          | Profit and loss, Applicable to orders which have a trade and aim to close position. It always is 0 in other conditions                                                                                                                                                                                                                                                                                                                                                                    |
| ordType                | String          | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order  <br>`optimal_limit_ioc`: Market order with immediate-or-cancel order<br>`mmp`: Market Maker Protection (only applicable to Option in Portfolio Margin mode)<br>`mmp_and_post_only`: Market Maker Protection and Post-only order(only applicable to Option in Portfolio Margin mode) <br>`op_fok`: Simple options (fok)<br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.) |
| side                   | String          | Order side                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| posSide                | String          | Position side                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| tdMode                 | String          | Trade mode                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| accFillSz              | String          | Accumulated fill quantity<br>The unit is `base_ccy` for SPOT and MARGIN, e.g. BTC-USDT, the unit is BTC; For market orders, the unit both is `base_ccy` when the tgtCcy is `base_ccy` or `quote_ccy`;<br>The unit is contract for `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                               |
| fillPx                 | String          | Last filled price. If none is filled, it will return "".                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| tradeId                | String          | Last traded ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| fillSz                 | String          | Last filled quantity<br>The unit is `base_ccy` for SPOT and MARGIN, e.g. BTC-USDT, the unit is BTC; For market orders, the unit both is `base_ccy` when the tgtCcy is `base_ccy` or `quote_ccy`;<br>The unit is contract for `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                    |
| fillTime               | String          | Last filled time                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| avgPx                  | String          | Average filled price. If none is filled, it will return "".                                                                                                                                                                                                                                                                                                                                                                                                                               |
| state                  | String          | State <br>`canceled`<br>` live` <br>`partially_filled`<br>`filled`<br>`mmp_canceled`                                                                                                                                                                                                                                                                                                                                                                                                      |
| stpId                  | String          | ~~Self trade prevention ID<br>Return "" if self trade prevention is not applicable~~  (Deprecated)                                                                                                                                                                                                                                                                                                                                                                                         |
| stpMode                | String          | Self trade prevention mode                                                                                                                                                                                                                                                                                                                                                                                                       |
| lever                  | String          | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                                                                                                                                                                                                                                                                              |
| attachAlgoClOrdId      | String          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| tpTriggerPx            | String          | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| tpTriggerPxType        | String          | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                      |
| tpOrdPx                | String          | Take-profit order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| isTpLimit              | String          | Whether the take-profit order is a limit order. `true` or `false`                                                                                                                                                                                                                                                                                                                                                                                                                         |
| slTriggerPx            | String          | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| slTriggerPxType        | String          | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                        |
| slOrdPx                | String          | Stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| attachAlgoOrds         | Array of objects | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > attachAlgoId         | String          | The order ID of attached TP/SL order. It can be used to identity the TP/SL order when amending. It will not be posted to algoId when placing the attached algo order after the general order is filled completely.                                                                                                                                                                                                                                                                                                                                             |
| > attachAlgoClOrdId    | String          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                                                                                                                                                                                                              |
| > tpTriggerPx          | String          | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > tpTriggerPxType      | String          | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                      |
| > tpOrdPx              | String          | Take-profit order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > slTriggerPx          | String          | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > slTriggerPxType      | String          | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                        |
| > slOrdPx              | String          | Stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > sz                   | String          | Size. Only applicable to TP order of split TPs                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > amendPxOnTriggerType | String          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                                                                                                                                                                                                                                            |
| linkedAlgoOrd  | Object          | Linked SL order detail, only applicable to the order that is placed by one-cancels-the-other (OCO) order that contains the TP limit order.                                                                                                                                                                                                                                                                                                                                                                                              |
| > algoId | String          | Algo ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| feeCcy                 | String          | Fee currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| fee                    | String          | Fee and rebate<br>For spot and margin, it is accumulated fee charged by the platform. It is always negative, e.g. -0.01. <br>For Expiry Futures, Perpetual Futures and Options, it is accumulated fee and rebate                                                                                                                                                                                                                                                                          |
| rebateCcy              | String          | Rebate currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| source                 | String          | Order source<br>`6`: The normal order triggered by the `trigger order`<br>`7`:The normal order triggered by the `TP/SL order` <br>`13`: The normal order triggered by the algo order<br>`25`:The normal order triggered by the `trailing stop order`                                                                                                                                                                                                                                      |
| rebate                 | String          | Rebate amount, only applicable to spot and margin, the reward of placing orders from the platform (rebate) given to user who has reached the specified trading level. If there is no rebate, this field is "".                                                                                                                                                                                                                                                                            |
| category               | String          | Category<br>`normal`<br>`twap` <br>`adl`<br>`full_liquidation`<br>`partial_liquidation`<br>`delivery`  <br>`ddh`: Delta dynamic hedge<br>`auto_conversion`                                                                                                                                                                                                                                                                                                                                                     |
| reduceOnly             | String          | Whether the order can only reduce the position size. Valid options: true or false.                                                                                                                                                                                                                                                                                                                                                                                                        |
| cancelSource           | String          | Code of the cancellation source.                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| cancelSourceReason     | String          | Reason for the cancellation.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| quickMgnType           | String          | ~~Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>`manual`, `auto_borrow`, `auto_repay`~~  (Deprecated)                                                                                                                                                                                                                                                                                                                                                     |
| algoClOrdId            | String          | Client-supplied Algo ID. There will be a value when algo order attaching `algoClOrdId` is triggered, or it will be "".                                                                                                                                                                                                                                                                                                                                                                    |
| algoId                 | String          | Algo ID. There will be a value when algo order is triggered, or it will be "".                                                                                                                                                                                                                                                                                                                                                                                                            |
| uTime                  | String          | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                  |
| cTime                  | String          | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                |
