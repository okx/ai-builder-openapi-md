---
site: okex
lang: en
name: POST / Place dca algo order
method: POST
path: /api/v5/tradingBot/dca/create
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Place dca algo order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/tradingBot/dca/create`

> Request Example

```shell
POST /api/v5/tradingBot/dca/create
body
{
    "instId": "BTC-USDT",
    "algoOrdType": "contract_dca",
    "direction": "long",
    "lever": "2",
    "initOrdAmt": "50",
    "maxSafetyOrds": "0",
    "safetyOrdAmt": "10",
    "pxSteps": "0.01",
    "tpPct": "0.05",
    "triggerParams": [
        {
            "triggerAction": "start",
            "triggerStrategy": "rsi",
            "timeframe": "30m",
            "thold": "10",
            "triggerCond": "cross",
            "timePeriod": "14"
        }
    ]
}
```

#### Request Parameters
| Parameter           | Type             | Required | Description                                                                                                                                                                                                             |
| :------------------ | :--------------- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| instId              | String           | Yes      | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                          |
| algoOrdType         | String           | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order                                                                                                                                     |
| initOrdAmt          | String           | Yes      | Initial order amount                                                                                                                                                                                                    |
| allowReinvest       | String           | No       | Whether to reinvest profit. Only applicable to Contract DCA<br>`true` or `false`, default is `true`                                                                                                                     |
| safetyOrdAmt        | String           | No       | Safety order amount<br>When `maxSafetyOrds` >= 1, `safetyOrdAmt` is required                                                                                                                                           |
| maxSafetyOrds       | String           | Yes      | Max number of safety orders                                                                                                                                                                                             |
| pxSteps             | String           | No       | Safety order price step<br>When `maxSafetyOrds` >= 1, `pxSteps` is required                                                                                                                                            |
| pxStepsMult         | String           | No       | Price step multiplier<br>When `maxSafetyOrds` >= 1, `pxStepsMult` is required                                                                                                                                          |
| volMult             | String           | No       | Safety order amount multiplier<br>When `maxSafetyOrds` >= 1, `volMult` is required                                                                                                                                     |
| tpPct               | String           | Yes      | Take-profit target per cycle<br>0.05 represents 5%                                                                                                                                                                     |
| slPct               | String           | No       | Stop-loss target<br>0.05 represents 5%                                                                                                                                                                                  |
| slMode              | String           | No       | Stop-loss mode<br>`limit`: Limit order<br>`market`: Market order                                                                                                                                                        |
| direction           | String           | No       | Contract DCA type. Only applicable to `contract_dca`<br>`long`: Long position, `short`: Short position                                                                                                                  |
| lever               | String           | Yes      | Leverage<br>Only applicable to `contract_dca`                                                                                                                                                                           |
| triggerParams       | Array of objects | Yes      | Trigger parameters                                                                                                                                                                                                      |
| > triggerAction     | String           | Yes      | Trigger action<br>Contract DCA: `start`: Start bot<br>Spot DCA: `start`: Start bot                                                                                                                                     |
| > triggerStrategy   | String           | Yes      | Trigger strategy<br>Contract DCA: `instant`: Instant trigger, `price`: Price trigger, `rsi`: RSI indicator trigger, default is `instant`<br>Spot DCA: `instant`: Instant trigger, `rsi`: RSI indicator trigger, default is `instant` |
| > timeframe         | String           | No       | K-line type<br>`3m`, `5m`, `15m`, `30m` (m: minute)<br>`1H`, `4H` (H: hour)<br>`1D` (D: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                                               |
| > thold             | String           | No       | Threshold<br>Integer between [1, 100]<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                       |
| > triggerCond       | String           | No       | Trigger condition<br>`cross_up`: Cross up<br>`cross_down`: Cross down<br>`above`: Above<br>`below`: Below<br>`cross`: Cross<br>This field is only valid when `triggerStrategy` is `rsi`                                 |
| > timePeriod        | String           | No       | Time period<br>`14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                                         |
| > triggerPx         | String           | No       | Trigger price<br>This field is only valid when `triggerStrategy` is `price`<br>Only applicable to `contract_dca`                                                                                                        |
| profitSharingRatio  | String           | No       | Lead trader profit sharing ratio. Only fixed profit sharing is supported. Only applicable to `contract_dca`<br>`0`, `0.1`, `0.2`, `0.3`                                                                                 |
| trackingMode        | String           | No       | Tracking mode. Only applicable to `contract_dca`<br>`sync`: Synchronous, `async`: Asynchronous                                                                                                                         |
| tag                 | String           | No       | Order tag                                                                                                                                                                                                               |
| algoClOrdId         | String           | No       | Client-supplied Algo ID                                                                                                                                                                                                 |
| tradeQuoteCcy       | String           | No       | Quote currency for trading. Only applicable to `spot_dca`                                                                                                                                                                 |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "447053782921515008",
            "sCode": "0",
            "sMsg": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter     | Type   | Description                                                   |
| :------------ | :----- | :------------------------------------------------------------ |
| algoId        | String | Algo ID                                                       |
| tag           | String | Order tag                                                     |
| algoClOrdId   | String | Client-supplied Algo ID                                       |
| sCode         | String | The code of the event execution result, 0 means success       |
| sMsg          | String | Rejection message if the request is unsuccessful              |
