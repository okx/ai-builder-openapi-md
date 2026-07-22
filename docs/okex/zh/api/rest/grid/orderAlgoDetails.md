---
site: okex
lang: zh
name: GET / 获取网格策略委托订单详情
method: GET
path: /api/v5/tradingBot/grid/orders-algo-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取网格策略委托订单详情 [order-book-trading-grid-trading-get-grid-algo-order-details]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/grid/orders-algo-details`

> 请求示例

```shell
GET /api/v5/tradingBot/grid/orders-algo-details?algoId=448965992920907776&algoOrdType=grid
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoOrdType | String | 是       | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托  |
| algoId    | String | 是       | 策略订单ID                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "actualLever": "",
            "activeOrdNum": "0",
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "algoOrdType": "grid",
            "annualizedRate": "0",
            "arbitrageNum": "0",
            "availEq": "",
            "basePos": false,
            "baseSz": "0",
            "cTime": "1681181054927",
            "cancelType": "1",
            "curBaseSz": "0",
            "curQuoteSz": "0",
            "direction": "",
            "eq": "",
            "floatProfit": "0",
            "gridNum": "10",
            "gridProfit": "0",
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "investment": "25",
            "lever": "0",
            "liqPx": "",
            "maxPx": "5000",
            "minPx": "400",
            "ordFrozen": "",
            "perMaxProfitRate": "1.14570215",
            "perMinProfitRate": "0.0991200440528634356837",
            "pnlRatio": "0",
            "profit": "0.00000000",
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
            "runPx": "30089.7",
            "singleAmt": "0.00101214",
            "slTriggerPx": "0",
            "state": "stopped",
            "stopResult": "0",
            "stopType": "1",
            "sz": "",
            "tag": "",
            "totalAnnualizedRate": "0",
            "totalPnl": "0",
            "tpTriggerPx": "0",
            "tradeNum": "0",
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
                    "triggerType": "manual",
                    "triggerTime": "1681181186484"
                }
            ],
            "uTime": "1681181186496",
            "uly": "",
            "profitSharingRatio": "",
            "copyType": "0",
            "tpRatio": "",
            "slRatio": "",
            "fee": "",
            "feeCcy": "",
            "fundingFee": "",
            "tradeQuoteCcy": "USDT"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**             | **类型** | **描述**                                                                                                                 |
|:--------------------| :------- |:-----------------------------------------------------------------------------------------------------------------------|
| algoId              | String   | 策略订单ID                                                                                                                 |
| algoClOrdId         | String   | 用户自定义策略ID                                                                                                              |
| instType            | String   | 产品类型                                                                                                                   |
| instId              | String   | 产品ID                                                                                                                   |
| cTime               | String   | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| uTime               | String   | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| algoOrdType         | String   | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托                                                                      |
| state               | String   | 订单状态<br>`starting`：启动中<br>`running`：运行中<br>`stopping`：终止中<br>`no_close_position`：已停止未平仓（仅适用于合约网格）<br>`stopped`：已停止     |
| rebateTrans         | Array of objects   | 返佣划转信息                                                                                                                 |
| > rebate            | String   | 返佣数量                                                                                                                   |
| > rebateCcy         | String   | 返佣币种                                                                                                                   |
| triggerParams       | Array of objects   | 信号触发参数                                                                                                                 |
| > triggerAction     | String   | 触发行为<br>`start`：网格启动<br>`stop`：网格停止                                                                                    |
| > triggerStrategy   | String   | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：rsi指标触发                                                                |
| > delaySeconds      | String   | 延迟触发时间，单位为秒                                                                                                            |
| > triggerTime       | String   | triggerAction实际触发时间，Unix时间戳的毫秒数格式, 如 `1597026383085`                                                                   |
| > triggerType       | String   | triggerAction的实际触发类型<br>`manual`：手动触发<br>`auto`: 自动触发                                                                  |
| > timeframe         | String   | K线种类<br>`3m`, `5m`, `15m`, `30m` (`m`代表分钟)<br>`1H`, `4H` (`H`代表小时)<br>`1D` (`D`代表天)<br>该字段只在`triggerStrategy`为`rsi`时有效 |
| > thold             | String   | 阈值<br>取值[1,100]的整数<br>该字段只在`triggerStrategy`为`rsi`时有效                                                                  |
| > triggerCond       | String   | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在`triggerStrategy`为`rsi`时有效  |
| > timePeriod        | String   | 周期<br>`14`<br>该字段只在`triggerStrategy`为`rsi`下有效                                                                          |
| > triggerPx         | String   | 触发价格<br>该字段只在`triggerStrategy`为`price`下有效                                                                              |
| > stopType          | String   | 策略停止类型<br>现货 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：停止平仓，`2`：停止不平仓 <br>该字段只在`triggerAction`为`stop`时有效                        |
| maxPx               | String   | 区间最高价格                                                                                                                 |
| minPx               | String   | 区间最低价格                                                                                                                 |
| gridNum             | String   | 网格数量                                                                                                                   |
| runType             | String   | 网格类型<br>`1`：等差，`2`：等比                                                                                                  |
| tpTriggerPx         | String   | 止盈触发价                                                                                                                  |
| slTriggerPx         | String   | 止损触发价                                                                                                                  |
| tradeNum            | String   | 挂单成交次数                                                                                                                 |
| arbitrageNum        | String   | 网格套利次数                                                                                                                 |
| singleAmt           | String   | 单网格买卖量                                                                                                                 |
| perMinProfitRate    | String   | 预期单网格最低利润率                                                                                                             |
| perMaxProfitRate    | String   | 预期单网格最高利润率                                                                                                             |
| runPx               | String   | 启动时价格                                                                                                                  |
| totalPnl            | String   | 总收益                                                                                                                    |
| pnlRatio            | String   | 收益率                                                                                                                    |
| investment          | String   | 累计投入金额<br>现货网格如果投入了交易币则折算为计价币                                                                                          |
| gridProfit          | String   | 网格利润                                                                                                                   |
| floatProfit         | String   | 浮动盈亏                                                                                                                   |
| totalAnnualizedRate | String   | 总年化                                                                                                                    |
| annualizedRate      | String   | 网格年化                                                                                                                   |
| cancelType          | String   | 网格策略停止原因<br>`0`：无<br>`1`：手动停止<br>`2`：止盈停止<br>`3`：止损停止<br>`4`：风控停止<br>`5`：交割停止<br>`6`: 信号停止                             |
| stopType            | String   | 网格策略停止类型<br>现货网格 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：市价全平，`2`：停止不平仓                                                       |
| activeOrdNum        | String   | 子订单挂单数量                                                                                                                |
| quoteSz             | String   | 计价币投入数量<br>仅适用于`现货网格`                                                                                                  |
| baseSz              | String   | 交易币投入数量<br>仅适用于`现货网格`                                                                                                  |
| curQuoteSz          | String   | 当前持有的计价币资产<br>仅适用于`现货网格`                                                                                               |
| curBaseSz           | String   | 当前持有的交易币资产<br>仅适用于`现货网格`                                                                                               |
| profit              | String   | 当前可提取利润,单位是计价币<br>仅适用于`现货网格`                                                                                           |
| stopResult          | String   | 策略停止结果<br>`0`：默认，`1`：市价卖币成功 `-1`：市价卖币失败<br>仅适用于`现货网格`                                                                  |
| direction           | String   | 合约网格类型<br>`long`：做多，`short`：做空，`neutral`：中性<br>仅适用于`合约网格`                                                              |
| basePos             | Boolean   | 是否开底仓<br>仅适用于`合约网格`                                                                                                    |
| sz                  | String   | 投入保证金，单位为`USDT`<br>仅适用于`合约网格`                                                                                          |
| lever               | String   | 杠杆倍数<br>仅适用于`合约网格`                                                                                                     |
| actualLever         | String   | 实际杠杆倍数<br>仅适用于`合约网格`                                                                                                   |
| liqPx               | String   | 预估强平价格<br>仅适用于`合约网格`                                                                                                   |
| uly                 | String   | 标的指数<br>仅适用于`合约网格`                                                                                                     |
| instFamily          | String   | 交易品种<br>适用于`交割`/`永续`/`期权`，如 `BTC-USD`<br>适用于`合约网格`                                                                         |
| ordFrozen           | String   | 挂单占用<br>适用于`合约网格`                                                                                                      |
| availEq             | String   | 可用保证金<br>适用于`合约网格`                                                                                                     |
| eq                  | String   | 策略账户总权益<br>仅适用于`合约网格`                                                                                                  |
| tag                 | String   | 订单标签                                                                                                                   |
| profitSharingRatio | String   | 分润比例<br>取值范围[0,0.3]<br>如果是普通订单（既不是带单也不是跟单），该字段返回""                                                                     |
| copyType           | String   | 分润订单类型<br>`0`：普通订单<br>`1`：普通跟单<br>`2`：分润跟单<br>`3`：带单                                                                   |
| tpRatio       | String          | 止盈比率，0.1 代表 10%                                                                                                        |
| slRatio     | String   | 止损比率，0.1 代表 10%                                                                                                        |
| fee       | String          | 累计手续费金额，仅适用于合约网格，其他网格策略为""                                                                                             |
|feeCcy       |	String	  | 累计手续费货币。仅适用于合约网格，其他网格策略为""                                                                                              |
| fundingFee     | String   | 累计资金费用，仅适用于合约网格，其他网格策略为""                                                                                              |
| tradeQuoteCcy   | String   | 用于交易的计价币种。 |
