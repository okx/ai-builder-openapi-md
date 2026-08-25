---
site: turkey
lang: en
name: Get / Order history (last 3 months)
method: GET
path: /api/v5/trade/orders-history-archive
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get / Order history (last 3 months)

Retrieve the completed order data of the last 3 months, and the incomplete orders that have been canceled are only reserved for 2 hours.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/orders-history-archive`

> Request Example

```shell
GET /api/v5/trade/orders-history-archive?ordType=post_only,fok,ioc&instType=SPOT

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                               |
| :-------- | :----- | :------- | :------------------------------------------------------------------------ |
| instType  | String | yes      | Instrument type<br>`SPOT`|
| instId    | String | No       | Instrument ID, e.g. `BTC-USDT`                                      |
| ordType   | String | No       | Order type <br>`market`: market order <br>`limit`: limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order <br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.)  |
| state     | String | No       | State<br>`canceled` <br>`filled`                                        |
| category  | String | No       |  Category <br>`twap`  |
| after     | String | No       | Pagination of data to return records earlier than the requested `ordId`   |
| before    | String | No       | Pagination of data to return records newer than the requested `ordId`     |
| limit     | String | No       | Number of results per request. The maximum is `100`; The default is `100` |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instType": "SPOT",
      "instId": "BTC-USDT",
      "ccy": "",
      "ordId": "312269865356374016",
      "clOrdId": "b1",
      "tag": "",
      "px": "999",
      "sz": "3",
      "ordType": "limit",
      "side": "buy",
      "posSide": "",
      "tdMode": "cash",
      "accFillSz": "0",
      "fillPx": "0",
      "tradeId": "0",
      "fillSz": "0",
      "fillTime": "0",
      "state": "filled",
      "avgPx": "0",
      "lever": "20",
      "tpTriggerPx": "",
      "tpTriggerPxType": "",
      "tpOrdPx": "",
      "slTriggerPx": "",
      "slTriggerPxType": "",
      "slOrdPx": "",
      "feeCcy": "",
      "fee": "",
      "rebateCcy": "",
      "rebate": "",
      "tgtCcy":"",
      "pnl": "",
      "category": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                            |
| :------------ | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| instType      | String   | Instrument type                                                                                                                                                            |
| instId        | String   | Instrument ID                                                                                                                                                              |
| tgtCcy        | String   | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell |
| ccy                    | String          | Margin currency <br>Applicable to all `isolated` `MARGIN` orders and `cross` `MARGIN` orders in `Futures mode`, `FUTURES` and `SWAP` contracts.                                                                                                                                                                                                                                                   |
| ordId         | String   | Order ID                                                                                                                                                                   |
| clOrdId       | String   | Client Order ID as assigned by the client                                                                                                                                                   |
| tag           | String   | Order tag                                                                                                                                                                  |
| px            | String   | Price                                                                                                                                                                      |
| sz            | String   | Quantity to buy or sell                                                                                                                                                    |
| ordType       | String   | Order type <br>`market`: market order <br>`limit`: limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order <br>`rpi`: Retail Price Improvement order<br>`elp`: Enhanced Liquidity Program order (Deprecated; use `rpi`. Accepted until October 31, 2026.)  |
| side          | String   | Order side                                                                                                                                                                 |
| posSide       | String   | Position side                                                                                                                                                              |
| tdMode        | String   | Trade mode                                                                                                                                                                 |
| accFillSz     | String   | Accumulated fill quantity                                                                                                                                                  |
| fillPx        | String   | Last filled price. If none is filled, it will return `0`.                                                                                                                  |
| tradeId       | String   | Last trade ID                                                                                                                                                              |
| fillSz        | String   | Last filled quantity                                                                                                                                                       |
| fillTime      | String   | Last filled time                                                                                                                                                           |
| avgPx         | String   | Average filled price. If none is filled, it will return "".                                                                                                              |
| state         | String   | State <br>`canceled` <br>`filled`                                                                                                                                          |
| lever         | String   | Leverage, from `0.01` to `125`                                                                                            |
| tpTriggerPx   | String   | Take-profit trigger price.                                                                                                            |
| tpTriggerPxType   | String   |Take-profit trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                             |
| tpOrdPx       | String   | Take-profit order price.                                                                                                              |
| slTriggerPx   | String   | Stop-loss trigger price.                                                                                                                 |
| slTriggerPxType   | String   |Stop-loss trigger price type. <br>`last`: last price<br>`index`: index price<br>`mark`: mark price                                                                                                             |
| slOrdPx       | String   | Stop-loss order price.                                                                                                                  |
| feeCcy        | String   | Fee currency                                                                                                                                                               |
| fee           | String   | Fee                                                                                                                                                                        |
| rebateCcy     | String   | Rebate currency                                                                                                                                                            |
| rebate        | String   | Rebate amount, the reward of placing orders from the platform (rebate) given to user who has reached the specified trading level. If there is no rebate, this field is "".  |
| pnl           | String   | Profit and loss                                                                                                                                                            |
| category      | String   | Category <br>`normal`<br>`twap`<br>`adl`<br>`full_liquidation`<br>`partial_liquidation`<br>`delivery`<br>`ddh`: Delta dynamic hedge<br>`auto_conversion` |
| uTime         | String   | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                   |
| cTime         | String   | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                 |
| stpId                  | String          | ~~Self trade prevention ID<br>Return "" if self trade prevention is not applicable~~ (Deprecated)                                                                                                                                                                                                                                                                                                                                                                                          |
| quickMgnType           | String   | ~~Quick Margin type, Only applicable to Quick Margin Mode of isolated margin<br>`manual`, `auto_borrow`, `auto_repay`~~  (Deprecated)                                                                                                                                                                                                                                                                                                                                                                    |
