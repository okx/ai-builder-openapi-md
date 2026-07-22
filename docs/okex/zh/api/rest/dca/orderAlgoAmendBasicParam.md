---
site: okex
lang: zh
name: POST / 现货DCA编辑参数
method: POST
path: /api/v5/tradingBot/dca/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 现货DCA编辑参数
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/dca/amend-order-algo`

> 请求示例

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

#### 请求参数
| 参数名        | 类型             | 是否必须 | 描述                                                                                                                                                        |
| :------------ | :--------------- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId        | String           | 是       | 策略ID                                                                                                                                                      |
| pxSteps       | String           | 是       | 价差比例（第一次加仓触发价格差）                                                                                                                            |
| pxStepsMult   | String           | 是       | 价差放大倍数                                                                                                                                                |
| volMult       | String           | 是       | 金额放大倍数                                                                                                                                                |
| tpPct         | String           | 是       | 止盈目标，0.05 表示 5%                                                                                                                                      |
| slPct         | String           | 是       | 止损目标，0.05 表示 5%                                                                                                              |
| initOrdAmt    | String           | 是       | 初始订单金额（计价货币）                                                                                                                                    |
| safetyOrdAmt  | String           | 是       | 加仓单金额（计价货币）                                                                                                                                      |
| maxSafetyOrds | String           | 是       | 最大加仓次数                                                                                                                                                |
| reserveFunds  | Boolean          | 是       | 是否预留全部资金<br>`true`：预留资金<br>`false`：不预留资金                                                                                         |
| triggerParams | Array of objects | 是       | 信号触发参数                                                                                                                                 |
| > triggerAction  | String        | 否       | 触发行为<br> `start`：马丁启动                                                                                                                          |
| > triggerStrategy| String        | 否       | 触发策略<br>`instant`：立即触发<br>`rsi`：RSI 指标触发                                                                                                      |
| > timeframe   | String           | 否       | K线种类<br>`3m`, `5m`, `15m`, `30m`（`m` 代表分钟）<br>`1H`, `4H`（`H` 代表小时）<br>`1D`（`D` 代表天）<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效    |
| > thold       | String           | 否       | 阈值，取值 [1, 100] 的整数<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                                                                  |
| > triggerCond | String           | 否       | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效        |
| > timePeriod  | String           | 否       | 周期，如 `14`<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                                                                               |                                                                                                                                                 |

> 返回结果

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

#### 返回参数
| 参数名      | 类型   | 描述                              |
| :---------- | :----- | :-------------------------------- |
| algoId      | String | 策略ID                            |
| algoClOrdId | String | 客户端自定义策略单ID              |
| sCode       | String | 事件执行结果的code，0代表成功     |
| sMsg        | String | 事件执行失败时的msg               |
