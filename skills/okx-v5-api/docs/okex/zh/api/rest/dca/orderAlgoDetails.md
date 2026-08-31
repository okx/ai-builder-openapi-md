---
site: okex
lang: zh
name: GET / 获取进行中马丁策略委托单列表
method: GET
path: /api/v5/tradingBot/dca/ongoing-list
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取进行中马丁策略委托单列表 [order-book-trading-dca-bot-trading-get-dca-bot-order-details-for-contract-dca]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/tradingBot/dca/ongoing-list`

> 请求示例

```shell
GET /api/v5/tradingBot/dca/ongoing-list?algoOrdType=contract_dca&limit=20
```

#### 请求参数
| 参数名      | 类型   | 是否必须 | 描述                                                                          |
| :---------- | :----- | :------- | :---------------------------------------------------------------------------- |
| algoOrdType | String | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托      |
| algoId      | String | 否       | 策略ID                                                                        |
| after       | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的 `algoId`             |
| before      | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的 `algoId`             |
| limit       | String | 否       | 返回结果的数量，最大为100，默认100条                                          |

> 返回结果

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

#### 返回参数
| 参数名             | 类型             | 描述                                                                                                                                                                                                    |
| :----------------- | :--------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| algoId             | String           | 策略订单 ID                                                                                                                                                                                             |
| algoOrdType        | String           | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托                                                                                                                                |
| instId             | String           | 产品 ID，如 `BTC-USDT-SWAP`                                                                                                                                                                             |
| copyType           | String           | 分润订单类型<br>`0`：普通订单<br>`1`：普通跟单<br>`2`：分润跟单<br>`3`：带单                                                                                                                            |
| state              | String           | 订单状态<br>`starting`：启动中<br>`running`：运行中<br>`stopping`：终止中<br>`pending_signal`：等待触发<br>`no_close_position`：已停止未平仓                                                              |
| direction          | String           | 合约马丁类型：`long`：多仓，`short`：空仓<br>现货马丁类型：`long`：做多                                                                                                                                |
| lever              | String           | 杠杆倍数<br>仅适用于 `contract_dca`                                                                                                                                                                     |
| initOrdAmt         | String           | 初始订单金额                                                                                                                                                                                            |
| safetyOrdAmt       | String           | 加仓单金额                                                                                                                                                                                              |
| maxSafetyOrds      | String           | 最大自动加仓次数                                                                                                                                                                                        |
| pxSteps            | String           | 跌多少加仓                                                                                                                                                                                              |
| pxStepsMult        | String           | 加仓价差倍数                                                                                                                                                                                            |
| volMult            | String           | 加仓金额倍数                                                                                                                                                                                            |
| tpPxRange          | String           | 止盈价格限制<br>做多时止盈价格不得低于系统最小阈值；做空时不得高于最大阈值<br>仅适用于 `contract_dca`                                                                                                   |
| slPct              | String           | 止损目标，如 `0.05` 表示 5%                                                                                                                                                                             |
| slMode             | String           | 止损模式<br>`limit`：限价<br>`market`：市价                                                                                                                                                             |
| allowReinvest      | Boolean          | 是否复投利润<br>`true` 或 `false`                                                                                                                                                                       |
| totalPnl           | String           | 总收益                                                                                                                                                                                                  |
| pnlRatio           | String           | 收益率                                                                                                                                                                                                  |
| totalFundingFee    | String           | 累计资金费用<br>仅适用于 `contract_dca`                                                                                                                                                                 |
| investmentAmt      | String           | 累计投入金额                                                                                                                                                                                            |
| investmentCcy      | String           | 投入数量单位，仅支持 `USDT`/`USDC`                                                                                                                                                                      |
| arbitragePnL       | String           | 周期套利收益                                                                                                                                                                                            |
| transferInMargin   | String           | 净转入金额，包括保证金和手动加仓金额<br>仅适用于 `contract_dca`                                                                                                                                         |
| profitSharingRatio | String           | 分润比例，取值范围 [0, 0.3]<br>普通订单返回 `""`<br>仅适用于 `contract_dca`                                                                                                                            |
| trackingMode       | String           | 分润设置<br>`sync`：同步<br>`async`：异步<br>仅适用于 `contract_dca`                                                                                                                                   |
| triggerParams      | Array of objects | 信号触发参数                                                                                                                                                                                            |
| > triggerAction    | String           | 触发行为<br>`start`：马丁启动<br>`stop`：马丁停止                                                                                                                                                       |
| > triggerStrategy  | String           | 触发策略<br>合约马丁类型：`instant`：立即触发，`price`：价格触发，`rsi`：RSI 指标触发，`webhook`：WS 信号触发<br>现货马丁类型：`instant`：立即触发，`rsi`：RSI 指标触发                                 |
| > triggerPx        | String           | 触发价格<br>仅在 `triggerStrategy` 为 `price` 时有效<br>仅适用于 `contract_dca`                                                                                                                        |
| > triggerCond      | String           | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>仅在 `triggerStrategy` 为 `rsi` 时有效                                                         |
| > timePeriod       | String           | 周期，如 `14`<br>仅在 `triggerStrategy` 为 `rsi` 时有效                                                                                                                                                |
| > thold            | String           | 阈值，取值 [1, 100] 的整数<br>仅在 `triggerStrategy` 为 `rsi` 时有效                                                                                                                                   |
| > timeframe        | String           | K 线种类<br>`3m`、`5m`、`15m`、`30m`（m 代表分钟）<br>`1H`、`4H`（H 代表小时）<br>`1D`（D 代表天）<br>仅在 `triggerStrategy` 为 `rsi` 时有效                                                           |
| cTime              | String           | 订单创建时间，Unix 时间戳毫秒数，如 `1597026383085`                                                                                                                                                     |
| uTime              | String           | 订单更新时间，Unix 时间戳毫秒数，如 `1597026383085`                                                                                                                                                     |
| ctVal              | String           | 合约面值<br>仅适用于 `contract_dca`                                                                                                                                                                     |
| tradeQuoteCcy      | String           | 指定交易计价货币<br>仅适用于 `spot_dca`                                                                                                                                                                 |                                                                                                            |
