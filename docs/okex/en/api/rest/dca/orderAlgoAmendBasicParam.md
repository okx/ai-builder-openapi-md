---
site: okex
lang: en
name: POST / Amend spot dca basic param
method: POST
path: /api/v5/tradingBot/dca/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend spot dca basic param
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/dca/amend-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/dca/amend-order-algo
body
{
    "algoId": "532177187189760000",
    "pxSteps": "0.02",
    "pxStepsMult": "2.0",
    "volMult": "2.0",
    "tpPct": "0.05",
    "slPct": "0.20",
    "initOrdAmt": "100",
    "safetyOrdAmt": "50",
    "maxSafetyOrds": "5",
    "reserveFunds": true,
    "triggerParams": [
        {
            "triggerAction": "start",
            "triggerStrategy": "instant"
        }
    ]
}
```

#### Request Parameters
| Parameter     | Type             | Required | Description                                                                                                                                                                          |
| :------------ | :--------------- | :------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId        | String           | Yes      | Algo order ID                                                                                                                                                                        |
| pxSteps       | String           | Yes      | Price step ratio (price gap to trigger the first safety order)                                                                                                                       |
| pxStepsMult   | String           | Yes      | Price step multiplier                                                                                                                                                                |
| volMult       | String           | Yes      | Amount multiplier                                                                                                                                                                    |
| tpPct         | String           | Yes      | Take-profit target, e.g. `0.05` represents 5%                                                                                                                                        |
| slPct         | String           | Yes      | Stop-loss target, e.g. `0.05` represents 5%                                                                                                                                          |
| initOrdAmt    | String           | Yes      | Initial order amount (in quote currency)                                                                                                                                             |
| safetyOrdAmt  | String           | Yes      | Safety order amount (in quote currency)                                                                                                                                              |
| maxSafetyOrds | String           | Yes      | Maximum number of safety orders                                                                                                                                                      |
| reserveFunds  | Boolean          | Yes      | Whether to reserve all funds<br>`true`: reserve funds<br>`false`: do not reserve funds                                                                                               |
| triggerParams | Array of objects | Yes      | Signal trigger parameters                                                                                                                                                            |
| > triggerAction  | String        | No       | Trigger action<br>`start`: start the DCA bot                                                                                                                                         |
| > triggerStrategy| String        | No       | Trigger strategy<br>`instant`: trigger immediately<br>`rsi`: RSI indicator trigger                                                                                                   |
| > timeframe   | String           | No       | Candlestick type<br>`3m`, `5m`, `15m`, `30m` (`m` = minutes)<br>`1H`, `4H` (`H` = hours)<br>`1D` (`D` = days)<br>Only valid when `triggerStrategy` is `rsi`                         |
| > thold       | String           | No       | Threshold, integer in range [1, 100]<br>Only valid when `triggerStrategy` is `rsi`                                                                                                   |
| > triggerCond | String           | No       | Trigger condition<br>`cross_up`: cross upward<br>`cross_down`: cross downward<br>`above`: above<br>`below`: below<br>`cross`: cross<br>Only valid when `triggerStrategy` is `rsi`    |
| > timePeriod  | String           | No       | Period, e.g. `14`<br>Only valid when `triggerStrategy` is `rsi`                                                                                                                      |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "532177187189760000",
            "algoClOrdId": "",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### Response Parameters
| Parameter   | Type   | Description                                          |
| :---------- | :----- | :--------------------------------------------------- |
| algoId      | String | Algo order ID                                        |
| algoClOrdId | String | Client-supplied algo order ID                        |
| sCode       | String | Event execution status code, `0` indicates success   |
| sMsg        | String | Error message if the event execution failed          |
