---
site: okex
lang: en
name: GET / DCA algo order details
method: GET
path: /api/v5/tradingBot/dca/ongoing-list
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / DCA algo order details
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`GET /api/v5/tradingBot/dca/ongoing-list`

> Request Example

```shell
GET /api/v5/tradingBot/dca/ongoing-list?algoOrdType=contract_dca&limit=20
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                                          |
| :---------- | :----- | :------- | :----------------------------------------------------------------------------------- |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order  |
| algoId      | String | No       | Algo ID                                                                              |
| after       | String | No       | Pagination of data to return records earlier than the requested `algoId`             |
| before      | String | No       | Pagination of data to return records newer than the requested `algoId`               |
| limit       | String | No       | Number of results per request. The maximum is 100. The default is 100                |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "565849588675117056",
            "algoOrdType": "contract_dca",
            "instId": "BTC-USDT-SWAP",
            "copyType": "0",
            "state": "running",
            "direction": "long",
            "lever": "3",
            "initOrdAmt": "100",
            "safetyOrdAmt": "200",
            "maxSafetyOrds": "5",
            "pxSteps": "0.02",
            "pxStepsMult": "1",
            "volMult": "1",
            "tpPxRange": "",
            "slPct": "",
            "slMode": "",
            "allowReinvest": true,
            "totalPnl": "12.5",
            "pnlRatio": "0.05",
            "totalFundingFee": "-0.5",
            "investmentAmt": "500",
            "investmentCcy": "USDT",
            "arbitragePnL": "2.1",
            "profitSharingRatio": "",
            "trackingMode": "",
            "triggerParams": [
                {
                    "triggerAction": "start",
                    "triggerStrategy": "instant"
                }
            ],
            "cTime": "1597026383085",
            "uTime": "1597026383085"
        }
    ]
}
```

#### Response Parameters
| Parameter          | Type             | Description                                                                                                                                                                                                              |
| :----------------- | :--------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId             | String           | Algo ID                                                                                                                                                                                                                  |
| algoOrdType        | String           | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order                                                                                                                                      |
| instId             | String           | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                     |
| copyType           | String           | Profit sharing order type<br>`0`: Normal order<br>`1`: Copy order without profit sharing<br>`2`: Copy order with profit sharing<br>`3`: Lead order                                                                       |
| state              | String           | Algo order state<br>`starting`<br>`running`<br>`stopping`<br>`pending_signal`<br>`no_close_position`: Stopped algo order but have not closed position yet                                                                |
| direction          | String           | Contract DCA: `long`: Long position, `short`: Short position<br>Spot DCA: `long`: Long position                                                                                                                          |
| lever              | String           | Leverage<br>Only applicable to `contract_dca`                                                                                                                                                                            |
| initOrdAmt         | String           | Initial order amount                                                                                                                                                                                                     |
| safetyOrdAmt       | String           | Safety order amount                                                                                                                                                                                                      |
| maxSafetyOrds      | String           | Max number of safety orders                                                                                                                                                                                              |
| pxSteps            | String           | Safety order price step                                                                                                                                                                                                  |
| pxStepsMult        | String           | Price step multiplier                                                                                                                                                                                                    |
| volMult            | String           | Safety order amount multiplier                                                                                                                                                                                           |
| tpPxRange          | String           | Take-profit price limit<br>For Long DCA, the take-profit price must not be lower than the minimum threshold; for Short DCA, the take-profit price must not exceed the maximum threshold<br>Only applicable to `contract_dca` |
| slPct              | String           | Stop-loss target, e.g. `0.05` represents 5%                                                                                                                                                                             |
| slMode             | String           | Stop-loss mode<br>`limit`: Limit order<br>`market`: Market order                                                                                                                                                         |
| allowReinvest      | Boolean          | Whether to reinvest profit<br>`true` or `false`                                                                                                                                                                          |
| totalPnl           | String           | Total PnL                                                                                                                                                                                                                |
| pnlRatio           | String           | PnL ratio                                                                                                                                                                                                                |
| totalFundingFee    | String           | Total funding fee<br>Only applicable to `contract_dca`                                                                                                                                                                   |
| investmentAmt      | String           | Total investment amount                                                                                                                                                                                                  |
| investmentCcy      | String           | The invested quantity unit, can only be `USDT`/`USDC`                                                                                                                                                                   |
| arbitragePnL       | String           | Arbitrage PnL                                                                                                                                                                                                            |
| transferInMargin   | String           | Net transfer in margin, including margin and manually added investment<br>Only applicable to `contract_dca`                                                                                                               |
| profitSharingRatio | String           | Profit sharing ratio, range [0, 0.3]<br>Returns `""` for normal orders<br>Only applicable to `contract_dca`                                                                                                             |
| trackingMode       | String           | Tracking mode<br>`sync`: Synchronous<br>`async`: Asynchronous<br>Only applicable to `contract_dca`                                                                                                                       |
| triggerParams      | Array of objects | Trigger parameters                                                                                                                                                                                                       |
| > triggerAction    | String           | Trigger action<br>`start`: Start bot<br>`stop`: Stop bot                                                                                                                                                                 |
| > triggerStrategy  | String           | Trigger strategy<br>Contract DCA: `instant`: Instant trigger, `price`: Price trigger, `rsi`: RSI indicator trigger, `webhook`: WebSocket signal trigger<br>Spot DCA: `instant`: Instant trigger, `rsi`: RSI indicator trigger |
| > triggerPx        | String           | Trigger price<br>This field is only valid when `triggerStrategy` is `price`<br>Only applicable to `contract_dca`                                                                                                         |
| > triggerCond      | String           | Trigger condition<br>`cross_up`: Cross up<br>`cross_down`: Cross down<br>`above`: Above<br>`below`: Below<br>`cross`: Cross<br>This field is only valid when `triggerStrategy` is `rsi`                                  |
| > timePeriod       | String           | Time period, e.g. `14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                                      |
| > thold            | String           | Threshold<br>Integer between [1, 100]<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                        |
| > timeframe        | String           | K-line type<br>`3m`, `5m`, `15m`, `30m` (m: minute)<br>`1H`, `4H` (H: hour)<br>`1D` (D: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                                               |
| cTime              | String           | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                     |
| uTime              | String           | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                     |
| ctVal              | String           | Contract value<br>Only applicable to `contract_dca`                                                                                                                                                                     |
| tradeQuoteCcy      | String           | Quote currency for trading<br>Only applicable to `spot_dca`                                                                                                                                                             |
