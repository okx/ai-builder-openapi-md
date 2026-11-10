---
site: okex
lang: zh
name: POST / 马丁策略委托下单
method: POST
path: /api/v5/tradingBot/dca/create
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 马丁策略委托下单 [order-book-trading-dca-trading-post-place-dca-algo-order]

#### 限速：20次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

#### HTTP请求
`POST /api/v5/tradingBot/dca/create`

> 请求示例

```shell
# 马丁下单
POST /api/v5/tradingBot/dca/create
body
{
	"instId": "BTC-USDT",
	"algoOrdType": "contract_dca",
	"direction": "long",
	"lever": "2",
    "initOrdAmt"="50",
    "maxSafetyOrds"="0",
    "safetyOrdAmt"="10",
    "pxSteps"="0.01",
    "tpPct"="0.05",
	"triggerParams":[
      {
         "triggerAction":"start",
         "triggerStrategy":"rsi",
         "timeframe":"30m",
         "thold":"10",
         "triggerCond":"cross",
         "timePeriod":"14"
      }
}

```

#### 请求参数
| 参数名              | 类型             | 是否必须  | 描述                                                                                                                                                                                              |
| :------------------ | :--------------- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| instId              | String           | 是       | 产品ID，如 `BTC-USDT`                                                                                                                                                                             |
| algoOrdType         | String           | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托                                                                                                                           |
| initOrdAmt          | String           | 是       | 初始订单金额                                                                                                                                                                                       |
| allowReinvest       | String           | 否       | 是否复投利润，仅适用于合约马丁<br>`true` 或者 `false`，默认为 `true`                                                                                                                               |
| safetyOrdAmt        | String           | 否       | 加仓单金额<br>当 `maxSafetyOrds` >= 1 时，`safetyOrdAmt` 必传                                                                                                                                     |
| maxSafetyOrds       | String           | 是       | 最大自动加仓次数                                                                                                                                                                                   |
| pxSteps             | String           | 否       | 跌多少加仓<br>当 `maxSafetyOrds` >= 1 时，`pxSteps` 必传                                                                                                                                          |
| pxStepsMult         | String           | 否       | 加仓价差倍数<br>当 `maxSafetyOrds` >= 1 时，`pxStepsMult` 必传                                                                                                                                    |
| volMult             | String           | 否       | 加仓金额倍数<br>当 `maxSafetyOrds` >= 1 时，`volMult` 必传                                                                                                                                        |
| tpPct               | String           | 是       | 单周期止盈目标<br>0.05 表示 5%                                                                                                                                                                     |
| slPct               | String           | 否       | 止损目标<br>0.05 表示 5%                                                                                                                                                                           |
| slMode              | String           | 否       | 止损模式<br>`limit`：限价<br>`market`：市价                                                                                                                                                        |
| direction           | String           | 否       | 合约马丁类型，仅适用于 `contract_dca`<br>`long`：多仓，`short`：空仓                                                                                                                              |
| lever               | String           | 是       | 杠杆倍数<br>仅适用于 `contract_dca`                                                                                                                                                                |
| triggerParams       | Array of objects | 是       | 信号触发参数                                                                                                                                                                                       |
| > triggerAction     | String           | 是       | 触发行为<br>合约马丁触发行为：`start`：马丁启动<br>现货马丁触发行为：`start`：马丁启动                                                                                                             |
| > triggerStrategy   | String           | 是       | 触发策略<br>合约马丁类型：`instant`：立即触发，`price`：价格触发，`rsi`：RSI 指标触发，默认为 `instant`<br>现货马丁类型：`instant`：立即触发，`rsi`：RSI 指标触发，默认为 `instant`                |
| > timeframe         | String           | 否       | K线种类<br>`3m`, `5m`, `15m`, `30m`（`m` 代表分钟）<br>`1H`, `4H`（`H` 代表小时）<br>`1D`（`D` 代表天）<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                           |
| > thold             | String           | 否       | 阈值<br>取值 [1,100] 的整数<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                                                                                                       |
| > triggerCond       | String           | 否       | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                              |
| > timePeriod        | String           | 否       | 周期<br>`14`<br>该字段只在 `triggerStrategy` 为 `rsi` 时有效                                                                                                                                      |
| > triggerPx         | String           | 否       | 触发价格<br>该字段只在 `triggerStrategy` 为 `price` 时有效<br>仅适用于 `contract_dca`                                                                                                             |
| profitSharingRatio  | String           | 否       | 带单员分润比例，仅支持固定比例分润，仅适用于 `contract_dca`<br>`0`, `0.1`, `0.2`, `0.3`                                                                                                           |
| trackingMode        | String           | 否       | 分润设置，仅适用于 `contract_dca`<br>`sync` 同步，`async` 异步                                                                                                                                    |
| tag                 | String           | 否       | 订单标签                                                                                                                                                                                           |
| algoClOrdId         | String           | 否       | 客户端自定义策略单ID                                                                                                                                                                               |
| tradeQuoteCcy       | String           | 否       | 指定交易计价货币，仅适用`spot_dca`                                                                                                                                                                    |

> 返回结果

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

#### 返回参数
| 参数名        | 类型   | 描述                              |
| :------------ | :----- | :-------------------------------- |
| algoId        | String | 策略订单ID                        |
| tag           | String | 订单标签                          |
| algoClOrdId   | String | 客户端自定义策略单ID              |
| sCode         | String | 事件执行结果的code，0代表成功     |
| sMsg          | String | 事件执行失败时的msg               |
