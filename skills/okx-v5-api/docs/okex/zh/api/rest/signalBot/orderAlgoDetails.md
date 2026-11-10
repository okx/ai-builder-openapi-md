---
site: okex
lang: zh
name: GET / 获取信号策略详情
method: GET
path: /api/v5/tradingBot/signal/orders-algo-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取信号策略详情 [order-book-trading-signal-bot-trading-get-signal-bot-order-details]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/orders-algo-details`

> 请求示例

```shell
GET /api/v5/tradingBot/signal/orders-algo-details?algoId=623833708424069120&algoOrdType=contract
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                        |
| :-------- | :----- | :------- |:--------------------------|
| algoOrdType | String | 是       | 策略类型<br>`contract`：合约信号 |
| algoId    | String | 是       | 策略ID                    |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "623833708424069120",
            "algoClOrdId": "",
            "algoOrdType": "contract",
            "availBal": "1.6561369013122267",
            "cTime": "1695005546360",
            "cancelType": "0",
            "entrySettingParam": {
                "allowMultipleEntry": true,
                "amt": "0",
                "entryType": "1",
                "ratio": ""
            },
            "exitSettingParam": {
                "slPct": "",
                "tpPct": "",
                "tpSlType": "price"
            },
            "floatPnl": "0.1279999999999927",
            "frozenBal": "25.16816",
            "instIds": [
                "BTC-USDT-SWAP",
                "ETH-USDT-SWAP"
            ],
            "instType": "SWAP",
            "investAmt": "100",
            "lever": "10",
            "ratio": "",
            "realizedPnl": "-73.303703098687766",
            "signalChanId": "623827579484770304",
            "signalChanName": "我的信号",
            "signalSourceType": "1",
            "state": "running",
            "subOrdType": "9",
            "totalEq": "26.824296901312227",
            "totalPnl": "-73.1757030986877733",
            "totalPnlRatio": "-0.7317570309868777",
            "uTime": "1697029422313"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**              | **类型**          | **描述**                                                                                                                                                         |
|:---------------------|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId               | String          | 策略ID                                                                                                                                                         |
| algoClOrdId          | String          | 用户自定义策略ID                                                                                                                                                      |
| instType             | String          | 产品类型                                                                                                                                                           |
| instIds              | Array of strings | 该信号支持的产品ID列表                                                                                                                                                   |
| cTime                | String          | 策略创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                       |
| uTime                | String          | 策略更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                       |
| algoOrdType          | String          | 策略类型<br>`contract`：合约信号                                                                                                                                      |
| state                | String          | 订单状态<br>`starting`：启动中<br>`running`：运行中<br>`stopping`：终止中<br>`stopped`：已停止                                                                                     |
| cancelType           | String          | 策略停止原因<br>`0`：无<br>`1`：手动停止                                                                                                                                    |
| totalPnl             | String          | 总收益                                                                                                                                                            |
| totalPnlRatio        | String          | 总收益率                                                                                                                                                            |
| totalEq              | String          | 当前策略总权益                                                                                                                                                        |
| floatPnl             | String          | 浮动盈亏                                                                                                                                                           |
| realizedPnl          | String          | 已实现盈亏                                                                                                                                                          |
| frozenBal            | String          | 占用保证金                                                                                                                                                          |
| availBal             | String          | 可用保证金                                                                                                                                                          |
| lever                | String          | 杠杆倍数<br>仅适用于`合约信号`                                                                                                                                             |
| investAmt            | String          | 投入金额                                                                                                                                                           |
| subOrdType           | String          | 委托类型<br>`1`：限价<br>`2`：市价<br>`9`：tradingView信号                                                                                                                  |
| ratio                | String          | 限价单的委托价格距离买一/卖一价的百分比<br>当委托类型为限价时，该字段有效，无效则返回""。                                                                                                               |
| entrySettingParam    | Object          | 进场参数设定                                                                                                                                                         |
| > allowMultipleEntry | Boolean          | 是否允许多次进场<br>`true`：允许<br>`false`：不允许                                                                                                                           |
| > entryType          | String          | 单次委托类型<br>`1`：单次委托量具体数值将从 TradingView 信号中传入<br>`2`：单次委托量为固定数量的保证金<br>`3`：单次委托量为固定的合约张数<br>`4`：单次委托量基于在收到触发信号时策略中可用保证金的百分比<br>`5`：单次委托量基于在创建策略时设置的初始投入保证金的百分比   |
| > amt                | String          | 单笔委托量<br>在单次委托类型是 固定保证金 / 合约张数 下该字段有效，无效的时候返回""                                                                                                                |
| > ratio              | String          | 单笔委托数量百分比<br>在单次委托类型是 占用保证金比例 / 初始投资比例 下该字段有效，无效的时候返回""                                                                                                        |
| exitSettingParam     | Object          | 离场参数设定                                                                                                                                                         |
| > tpSlType           | String          | 止盈止损类型，该参数用户确定设置止盈止损的触发价格计算的方式<br>`pnl`：基于平均持仓成本和预期收益率<br>`price`：基于相对于平均持仓成本的涨跌幅                                                                              |
| > tpPct              | String          | 止盈百分比                                                                                                                                                          |
| > slPct              | String          | 止损百分比                                                                                                                                                          |
| signalChanId         | String          | 信号ID                                                                                                                                                           |
| signalChanName       | String          | 信号名称                                                                                                                                                           |
| signalSourceType     | String          | 信号来源类型<br>`1`：自己创建的<br>`2`：订阅他人<br>`3`：免费信号                                                                                                                    |
