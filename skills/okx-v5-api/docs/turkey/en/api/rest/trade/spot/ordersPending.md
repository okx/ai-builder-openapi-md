---
site: turkey
lang: en
name: GET / Order List
method: GET
path: /api/v5/trade/orders-pending
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Order List

Retrieve all incomplete orders under the current account.

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/orders-pending`

> Request Example

```shell
GET /api/v5/trade/orders-pending?ordType=post_only,fok,ioc&instType=SPOT

```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve all incomplete orders
result = tradeAPI.get_order_list(
    instType="SPOT",
    ordType="post_only,fok,ioc"
)
print(result)
```

#### Request Parameters

| Parameter       | Type   | Required | Description                                                                                                                                                       |
|:----------------| :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType        | String | No       | Instrument type<br>`SPOT`                                                                                                                                         |
| instId          | String | No       | Instrument ID, e.g. `BTC-USDT`                                                                                                                                    |
| ordType         | String | No       | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order<br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.) |
| state           | String | No       | State<br>`live`<br>`partially_filled`                                                                                                                             |
| after           | String | No       | Pagination of data to return records earlier than the requested `ordId`                                                                                           |
| before          | String | No       | Pagination of data to return records newer than the requested `ordId`                                                                                             |
| limit           | String | No       | Number of results per request. The maximum is `100`. The default is `100`                                                                                         |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "",
            "cTime": "1724733617998",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "0",
            "feeCcy": "BTC",
            "fillPx": "",
            "fillSz": "0",
            "fillTime": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "1752588852617379840",
            "ordType": "post_only",
            "pnl": "0",
            "posSide": "net",
            "px": "13013.5",
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
            "state": "live",
            "stpId": "",
            "stpMode": "cancel_maker",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "",
            "uTime": "1724733617998"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**          | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|:-----------------------| :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType               | String   | Instrument type                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| instId                 | String   | Instrument ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| tgtCcy                 | String   | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                         |
| ccy                    | String          | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`, `FUTURES` and `SWAP` contracts.                                                                                                                                                                                                                                                   |
| ordId                  | String   | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| clOrdId                | String   | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| tag                    | String   | Order tag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| px                     | String   | Price <br>For options, use coin as unit (e.g. BTC, ETH)                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| pxUsd                  | String | Options price in USDOnly applicable to options; return "" for other instrument types                                                                                                                                                                                                                                                                                                                                                                                                       |
| pxVol                  | String | Implied volatility of the options orderOnly applicable to options; return "" for other instrument types                                                                                                                                                                                                                                                                                                                                                                                    |
| pxType                 | String | Price type of options <br>`px`: Place an order based on price, in the unit of coin (the unit for the request parameter px is BTC or ETH) <br>`pxVol`: Place an order based on pxVol <br>`pxUsd`: Place an order based on pxUsd, in the unit of USD (the unit for the request parameter px is USD)                                                                                                                                                                                          |
| sz                     | String   | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| pnl                    | String   | Profit and loss, Applicable to orders which have a trade and aim to close position. It always is 0 in other conditions                                                                                                                                                                                                                                                                                                                                                                     |                                    |
| ordType                | String   | Order type <br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order  <br>`optimal_limit_ioc`: Market order with immediate-or-cancel order<br>`mmp`: Market Maker Protection (only applicable to Option in Portfolio Margin mode)<br>`mmp_and_post_only`: Market Maker Protection and Post-only order(only applicable to Option in Portfolio Margin mode) <br>`op_fok`: Simple options (fok)<br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.) |
| side                   | String   | Order side                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| posSide                | String   | Position side                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| tdMode                 | String   | Trade mode                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| accFillSz              | String   | Accumulated fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| fillPx                 | String   | Last filled price                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| tradeId                | String   | Last trade ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| fillSz                 | String   | Last filled quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| fillTime               | String   | Last filled time                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| avgPx                  | String   | Average filled price. If none is filled, it will return "".                                                                                                                                                                                                                                                                                                                                                                                                                                |
| state                  | String   | State<br>`live` <br>`partially_filled`                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| lever                  | String   | Leverage, from `0.01` to `125`. <br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                                                                                                                                                                                                                                                                               |
| attachAlgoClOrdId      | String   | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| tpTriggerPx            | String   | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| tpTriggerPxType        | String   | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                       |
| tpOrdPx                | String   | Take-profit order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| slTriggerPx            | String   | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| slTriggerPxType        | String   | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                         |
| slOrdPx                | String   | Stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| attachAlgoOrds         | Array of objects   | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > attachAlgoId         | String   | The order ID of attached TP/SL order. It can be used to identity the TP/SL order when amending. It will not be posted to algoId when placing the attached algo order after the general order is filled completely.                                                                                                                                                                                                                                                                                                                                               |
| > attachAlgoClOrdId    | String   | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                                                                                                                                                                                                               |
| > tpTriggerPx          | String   | Take-profit trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > tpTriggerPxType      | String   | Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                       |
| > tpOrdPx              | String   | Take-profit order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > slTriggerPx          | String   | Stop-loss trigger price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > slTriggerPxType      | String   | Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                                                                                                                                                                                                                                                                                                         |
| > slOrdPx              | String   | Stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > sz                   | String | Size. Only applicable to TP order of split TPs                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > amendPxOnTriggerType | String            | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                                                                                                                                                                                                                                             |
| stpId                  | String   | ~~Self trade prevention ID<br>Return "" if self trade prevention is not applicable~~  (Deprecated)                                                                                                                                                                                                                                                                                                                                                                                          |
| stpMode                | String   | Self trade prevention mode                                                                                                                                                                                                                                                                                                                                                                                                        |
| feeCcy                 | String   | Fee currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| fee                    | String   | Fee and rebate<br>For spot and margin, it is accumulated fee charged by the platform. It is always negative, e.g. -0.01. <br>For Expiry Futures, Perpetual Futures and Options, it is accumulated fee and rebate                                                                                                                                                                                                                                                                           |
| rebateCcy              | String   | Rebate currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| source                 | String   | Order source<br>`6`: The normal order triggered by the `trigger order`<br>`7`:The normal order triggered by the `TP/SL order` <br>`13`: The normal order triggered by the algo order<br>`25`:The normal order triggered by the `trailing stop order`                                                                                                                                                                                                                                       |
| rebate                 | String   | Rebate amount, only applicable to spot and margin, the reward of placing orders from the platform (rebate) given to user who has reached the specified trading level. If there is no rebate, this field is "".                                                                                                                                                                                                                                                                             |
| category               | String   | Category <br>`normal`                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| reduceOnly             | String   | Whether the order can only reduce the position size. Valid options: true or false.                                                                                                                                                                                                                                                                                                                                                                                                         |
| quickMgnType           | String   | ~~Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>`manual`, `auto_borrow`, `auto_repay`~~  (Deprecated)                                                                                                                                                                                                                                                                                                                                                      |
| algoClOrdId            | String | Client-supplied Algo ID. There will be a value when algo order attaching `algoClOrdId` is triggered, or it will be "".                                                                                                                                                                                                                                                                                                                                                                     |
| algoId                 | String | Algo ID. There will be a value when algo order is triggered, or it will be "".                                                                                                                                                                                                                                                                                                                                                                                                             |
| uTime                  | String   | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                   |
| cTime                  | String   | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                 |
| cancelSource           | String          | Code of the cancellation source.                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| cancelSourceReason     | String          | Reason for the cancellation.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
