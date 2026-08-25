---
site: okex
lang: en
name: GET / Grid algo order list
method: GET
path: /api/v5/tradingBot/grid/orders-algo-pending
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Grid algo order list

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/grid/orders-algo-pending`

> Request Example

```shell
GET /api/v5/tradingBot/grid/orders-algo-pending?algoOrdType=grid
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoOrdType | String | Yes       | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid   |
| algoId    | String | No       | Algo ID                                                   |
| instId    | String | No       | Instrument ID, e.g. `BTC-USDT`                                                   |
| instType    | String | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`FUTURES`<br>`SWAP`  |
| after    | String | No       | Pagination of data to return records earlier than the requested `algoId`.  |
| before    | String | No       | Pagination of data to return records newer than the requested `algoId`.  |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "actualLever": "",
            "algoClOrdId": "",
            "algoId": "56802********64032",
            "algoOrdType": "grid",
            "arbitrageNum": "0",
            "availEq": "",
            "basePos": false,
            "baseSz": "0",
            "cTime": "1681700496249",
            "cancelType": "0",
            "direction": "",
            "floatProfit": "0",
            "gridNum": "10",
            "gridProfit": "0",
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "investment": "25",
            "lever": "",
            "liqPx": "",
            "maxPx": "5000",
            "minPx": "400",
            "ordFrozen": "",
            "pnlRatio": "0",
            "quoteSz": "25",
            "rebateTrans": [
                {
                    "rebate": "0",
                    "rebateCcy": "BTC"
                },
                {
                    "rebate": "0",
                    "rebateCcy": "USDT"
                }
            ],
            "runType": "1",
            "slTriggerPx": "",
            "state": "running",
            "stopType": "",
            "sz": "",
            "tag": "",
            "totalPnl": "0",
            "tpTriggerPx": "",
            "triggerParams": [
                {
                    "triggerAction": "start",
                    "delaySeconds": "0",
                    "triggerStrategy": "instant",
                    "triggerType": "auto",
                    "triggerTime": ""
                },
                {
                    "triggerAction": "stop",
                    "delaySeconds": "0",
                    "triggerStrategy": "instant",
                    "stopType": "1",
                    "triggerPx": "1000",
                    "triggerType": "manual",
                    "triggerTime": ""
                }
            ],
            "uTime": "1682062564350",
            "uly": "BTC-USDT",
            "profitSharingRatio": "",
            "copyType": "0",
            "fee": "",
            "feeCcy": "",
            "fundingFee": "",
            "tradeQuoteCcy": "USDT"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**       | **Type** | **Description**                                                                                                                                                                                        |
|:--------------------| :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId              | String   | Algo ID                                                                                                                                                                                                |
| algoClOrdId         | String   | Client-supplied Algo ID                                                                                                                                                                                |
| instType            | String   | Instrument type                                                                                                                                                                                        |
| instId              | String   | Instrument ID                                                                                                                                                                                          |
| cTime               | String   | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |
| uTime               | String   | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |
| algoOrdType         | String   | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                                                                                 |
| state               | String   | Algo order state<br>`starting`<br>`running`<br>`stopping`<br>`pending_signal`<br>`no_close_position`: stopped algo order but have not closed position yet                                                                  |
| rebateTrans         | Array of objects   | Rebate transfer info                                                                                                                                                                                   |
| > rebate            | String   | Rebate amount                                                                                                                                                                                          |
| > rebateCcy         | String   | Rebate currency                                                                                                                                                                                        |
| triggerParams       | Array of objects   | Trigger Parameters                                                                                                                                                                                     |
| > triggerAction     | String   | Trigger action<br>`start`<br>`stop`                                                                                                                                                                    |
| > triggerStrategy   | String   | Trigger strategy<br>`instant`<br>`price`<br>`rsi`                                                                                                                                                      |
| > delaySeconds      | String   | Delay seconds after action triggered                                                                                                                                                                   |
| > triggerTime       | String   | Actual action triggered time, unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                              |
| > triggerType       | String   | Actual action triggered type<br>`manual`<br>`auto`                                                                                                                                                     |
| > timeframe         | String   | K-line type<br>`3m`, `5m`, `15m`, `30m` (`m`: minute)<br>`1H`, `4H` (`H`: hour)<br>`1D` (`D`: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                         |
| > thold             | String   | Threshold<br>The value should be an integer between 1 to 100<br>This field is only valid when `triggerStrategy` is `rsi`                                                                               |
| > triggerCond       | String   | Trigger condition<br>`cross_up`<br>`cross_down`<br>`above`<br>`below`<br>`cross`<br>This field is only valid when `triggerStrategy` is `rsi`                                                           |
| > timePeriod        | String   | Time Period<br>`14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                        |
| > triggerPx         | String   | Trigger Price<br>This field is only valid when `triggerStrategy` is `price`                                                                                                                            |
| > stopType          | String   | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions<br>This field is only valid when `triggerAction` is `stop` |
| maxPx               | String   | Upper price of price range                                                                                                                                                                             |
| minPx               | String   | Lower price of price range                                                                                                                                                                             |
| gridNum             | String   | Grid quantity                                                                                                                                                                                          |
| runType             | String   | Grid type<br>`1`: Arithmetic, `2`: Geometric                                                                                                                                                           |
| tpTriggerPx         | String   | Take-profit trigger price                                                                                                                                                                              |
| slTriggerPx         | String   | Stop-loss trigger price                                                                                                                                                                                |
| arbitrageNum        | String   | The number of arbitrages executed                                                                                                                                                                      |
| totalPnl            | String   | Total P&L                                                                                                                                                                                              |
| pnlRatio            | String   | P&L ratio                                                                                                                                                                                              |
| investment          | String   | Accumulated investment amount<br>Spot grid investment amount calculated on quote currency                                                                                                              |
| gridProfit          | String   | Grid profit                                                                                                                                                                                            |
| floatProfit         | String   | Variable P&L                                                                                                                                                                                           |
| cancelType          | String   | Algo order stop reason<br>`0`: None<br>`1`: Manual stop<br>`2`: Take profit<br>`3`: Stop loss<br>`4`: Risk control<br>`5`: Delivery<br>`6`: Signal                                                     |
| stopType            | String   | Actual Stop type<br>Spot `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions                                                          |
| quoteSz             | String   | Quote currency investment amount<br>Only applicable to `Spot grid`                                                                                                                                     |
| baseSz              | String   | Base currency investment amount<br>Only applicable to `Spot grid`                                                                                                                                      |
| direction           | String   | Contract grid type<br>`long`,`short`,`neutral`<br>Only applicable to `contract grid`                                                                                                                   |
| basePos             | Boolean   | Whether or not to open a position when the strategy is activated<br>Only applicable to `contract grid`                                                                                                 |
| sz                  | String   | Used margin based on `USDT`<br>Only applicable to `contract grid`                                                                                                                                      |
| lever               | String   | Leverage<br>Only applicable to `contract grid`                                                                                                                                                         |
| actualLever         | String   | Actual Leverage<br>Only applicable to `contract grid`                                                                                                                                                  |
| liqPx               | String   | Estimated liquidation price<br>Only applicable to `contract grid`                                                                                                                                      |
| uly                 | String   | Underlying<br>Only applicable to `contract grid`                                                                                                                                                       |
| instFamily          | String   | Instrument family<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`<br>Only applicable to `contract grid`                                                                                                |
| ordFrozen           | String   | Margin used by pending orders<br>Only applicable to `contract grid`                                                                                                                                    |
| availEq             | String   | Available margin<br>Only applicable to `contract grid`                                                                                                                                                 |
| tag                 | String   | Order tag                                                                                                                                                                                              |
| profitSharingRatio  | String   | Profit sharing ratio<br>Value range [0, 0.3]<br>If it is a normal order (neither copy order nor lead order), this field returns ""                                                                     |
| copyType            | String   | Profit sharing order type<br>`0`: Normal order<br>`1`: Copy order without profit sharing<br>`2`: Copy order with profit sharing<br>`3`: Lead order                                                     |
| fee       | String          | Accumulated fee. Only applicable to contract grid, or it will be ""                                                                                                                                    |
| feeCcy	| String	| Accumulated fee currency. Only applicable to contract grid, or it will be ""                                                                                                                                  |
| fundingFee     | String   | Accumulated funding fee. Only applicable to contract grid, or it will be ""                                                                                                                            |
| tradeQuoteCcy   | String   | The quote currency for trading. |
