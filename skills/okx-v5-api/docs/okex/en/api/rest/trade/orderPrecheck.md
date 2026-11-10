---
site: okex
lang: en
name: POST / Order precheck
method: POST
path: /api/v5/trade/order-precheck
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Order precheck

This endpoint is used to precheck the account information before and after placing the order. <br>
Only applicable to `Multi-currency margin mode`, and `Portfolio margin mode`.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/order-precheck`

> Request Example

```shell
# place order for SPOT
POST /api/v5/trade/order-precheck
 body
 {
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "clOrdId":"b15",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}
```

#### Request Parameters

| Parameter              | Type    | Required    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|:-----------------------| :------ |:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                 | String  | Yes         | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| tdMode                 | String  | Yes         | Trade mode<br>Margin mode `cross` `isolated`<br>Non-Margin mode `cash`<br>`spot_isolated` (only applicable to SPOT lead trading, `tdMode` should be `spot_isolated` for `SPOT` lead trading.)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| side                   | String  | Yes         | Order side, `buy` `sell`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| posSide                | String  | Conditional | Position side  <br>The default is `net` in the `net` mode <br>It is required in the `long/short` mode, and can only be `long` or `short`. <br>Only applicable to `FUTURES`/`SWAP`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ordType                | String  | Yes         | Order type <br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order  <br>`optimal_limit_ioc`: Market order with immediate-or-cancel order (applicable only to Expiry Futures and Perpetual Futures).  <br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.)                                                                                                                                                                                                                                                 |
| sz                     | String  | Yes         | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| px                     | String  | Conditional | Order price. Only applicable to `limit`,`post_only`,`fok`,`ioc`,`mmp`,`mmp_and_post_only` order.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| outcome | String | Conditional | The market outcome users trade on.<br>`yes`<br>`no`<br>Only applicable and required for `EVENTS` |
| reduceOnly             | Boolean | No          | Whether orders can only reduce in position size.  <br>Valid options: `true` or `false`. The default value is `false`.<br>Only applicable to `MARGIN` orders, and `FUTURES`/`SWAP` orders in `net` mode <br>Only applicable to `Futures mode` and `Multi-currency margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| tgtCcy                 | String  | No          | Whether the target currency uses the quote or base currency.<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| attachAlgoOrds         | Array of objects   | No          | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > attachAlgoClOrdId    | String   | No          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > tpTriggerPx          | String | Conditional | Take-profit trigger price<br>For condition TP order, if you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| > tpOrdPx              | String | Conditional | Take-profit order price <br><br>For condition TP order, if you fill in this parameter, you should fill in the take-profit trigger price as well. <br>For limit TP order, you need to fill in this parameter, take-profit trigger needn‘t to be filled. <br>If the price is -1, take-profit will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > tpOrdKind | String | No       | TP order kind<br>`condition`<br>`limit`<br> The default is `condition`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > slTriggerPx          | String | Conditional | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > slOrdPx              | String | Conditional | Stop-loss order price<br>If you fill in this parameter, you should fill in the stop-loss trigger price.<br>If the price is -1, stop-loss will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > tpTriggerPxType      | String | No          | Take-profit trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price <br>The default is last                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > slTriggerPxType      | String | No          | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price <br>The default is last                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > sz                   | String | Conditional | Size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > callbackRatio       | String | Conditional | Callback ratio, e.g. `0.05` represents 5%.<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > callbackSpread      | String | Conditional | Callback spread (price distance).<br>Either `callbackRatio` or `callbackSpread` is required. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > activePx            | String | No          | Activation price.<br>The trailing stop is activated when the market price reaches the activation price. After activation, the system starts calculating the actual trigger price. If not provided, the trailing stop is activated immediately upon order placement.<br>Only applicable when `ordType` = `move_order_stop` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "41.94347460746277",
            "adjEqChg": "-226.05616481626",
            "availBal": "0",
            "availBalChg": "0",
            "imr": "0",
            "imrChg": "57.74709688430927",
            "liab": "0",
            "liabChg": "0",
            "liabChgCcy": "",
            "liqPx": "6764.8556232031115",
            "liqPxDiff": "-57693.044376796888536773622035980224609375",
            "liqPxDiffRatio": "-0.8950500152315991",
            "mgnRatio": "0",
            "mgnRatioChg": "0",
            "mmr": "0",
            "mmrChg": "0",
            "posBal": "",
            "posBalChg": "",
            "type": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**    |
| :------------ | :------- | :--------------------------------------------------------- |
| adjEq            | String     | Current adjusted / Effective equity in `USD`                                                                                      |
| adjEqChg         | String     | After placing order, changed quantity of adjusted / Effective equity in `USD`                                                                              |
| imr              | String     | Current initial margin requirement in `USD`                                                                                      |
| imrChg           | String     | After placing order, changed quantity of initial margin requirement in `USD`                                                                             |
| mmr              | String     | Current Maintenance margin requirement in `USD`                                                                                      |
| mmrChg           | String     | After placing order, changed quantity of maintenance margin requirement in `USD`                                                                              |
| mgnRatio         | String     | Current Maintenance margin ratio in `USD`                                                                                       |
| mgnRatioChg      | String     | After placing order, changed quantity of Maintenance margin ratio in `USD`                                                                                |
| availBal         | String     | Current available balance in margin coin currency, only applicable to turn auto borrow off                                                                        |
| availBalChg         | String     | After placing order, changed quantity of available balance after placing order, only applicable to turn auto borrow off                                                                  |
| liqPx         | String     | Current estimated liquidation price                                                                                          |
| liqPxDiff         | String     | After placing order, the distance between estimated liquidation price and mark price                                                                               |
| liqPxDiffRatio         | String     | After placing order, the distance rate between estimated liquidation price and mark price                                                                              |
| posBal         | String     | Current positive asset, only applicable to margin isolated position                                                                            |
| posBalChg         | String     | After placing order, positive asset of margin isolated, only applicable to margin isolated position                                                                       |
| liab         | String     | Current liabilities of currency<br> For cross, it is cross liabilities<br>For isolated position, it is isolated liabilities                                                             |
| liabChg         | String     | After placing order, changed quantity of liabilities<br> For cross, it is cross liabilities<br>For isolated position, it is isolated liabilities                                                    |
| liabChgCcy         | String     | After placing order, the unit of changed liabilities quantity<br> only applicable cross and in auto borrow                                                                 |
| type         | String     | Unit type of positive asset, only applicable to margin isolated position<br>`1`: it is both base currency before and after placing order <br> `2`: before plaing order, it is base currency. after placing order, it is quota currency.<br>`3`: before plaing order, it is quota currency. after placing order, it is base currency<br> `4`: it is both quota currency before and after placing order |
