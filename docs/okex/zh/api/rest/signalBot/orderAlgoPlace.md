---
site: okex
lang: zh
name: POST / 创建信号策略
method: POST
path: /api/v5/tradingBot/signal/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 创建信号策略 [order-book-trading-signal-bot-trading-post-create-signal-bot]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/order-algo`

> 请求示例

```shell
# 创建信号策略
POST /api/v5/tradingBot/signal/order-algo
body
{
  "signalChanId": "627921182788161536",
  "instIds": [
    "BTC-USDT-SWAP",
    "ETH-USDT-SWAP",
    "LTC-USDT-SWAP"
  ],
  "lever": "10",
  "investAmt": "100",
  "subOrdType": "9",
  "entrySettingParam": {
    "allowMultipleEntry": true,
    "entryType": "1",
    "amt": "",
    "ratio": ""
  },
  "exitSettingParam": {
    "tpSlType": "2",
    "tpPct": "",
    "slPct": ""
  }
}
```

#### 请求参数

| 参数名                   | 类型               | 是否必须 | 描述                                                                                                                                                            |
|:----------------------|:-----------------|:-----|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| signalChanId          | String           | 是    | 信号ID                                                                                                                                                          |
| includeAll            | Boolean          | 否    | 是否包含所有USDT 本位永续合约，默认false。 `true`: 包含 `false` : 不包含                                                                                                           |
| instIds               | String           | 否    | 该信号支持的产品ID列表， 多个instId 用逗号分隔。当 includeAll 为true 时， 忽略此参数                                                                                                      |
| lever                 | String           | 是    | 杠杆倍数仅适用于合约信号                                                                                                                                                  |
| investAmt             | String           | 是    | 投入金额                                                                                                                                                          |
| subOrdType            | String           | 是    | 1：限价 2：市价 9：由tradingView信号指定                                                                                                                                  |
| ratio                 | String           | 否    | 限价单的委托价格距离买一/卖一价的百分比。当委托类型为限价时，该字段有效。                                                                                                                         |
| entrySettingParam     | String           | 否    | 进场参数设定                                                                                                                                                        |
| > allowMultipleEntry  | String           | 否    | 是否允许多次进场，默认允许。 `true`：允许 `false`：不允许                                                                                                                          |
| > entryType          | String          | 否    | 单次委托类型<br>`1`：单次委托量具体数值将从 TradingView 信号中传入<br>`2`：单次委托量为固定数量的保证金<br>`3`：单次委托量为固定的合约张数<br>`4`：单次委托量基于在收到触发信号时策略中可用保证金的百分比<br>`5`：单次委托量基于在创建策略时设置的初始投入保证金的百分比  |
| > amt                 | String           | 否    | 单笔委托量<br>在单次委托类型是 固定保证金 / 合约张数 下该字段有效                                                                                                                         |
| > ratio               | Array of objects  | 否    | 单笔委托数量百分比<br>在单次委托类型是 占用保证金比例 / 初始投资比例 下该字段有效                                                                                                                                    |
| exitSettingParam      | String           | 否    | 离场参数设定                                                                                                                           |
| > tpSlType            | String           | 是    | 止盈止损类型，该参数用户确定设置止盈止损的触发价格计算的方式<br>`pnl`：基于平均持仓成本和预期收益率<br>`price`：基于相对于平均持仓成本的涨跌幅                                                                                       |
| > tpPct               | String           | 否    | 止盈百分比                                                                                                                                            |
| > slPct               | String           | 否    | 止损百分比                                        |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "447053782921515008",
            "sCode": "0",
            "sMsg": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**            |
|:------------| :------- |:------------------|
| algoId      | String   | 策略ID            |
| algoClOrdId | String   | 用户自定义策略ID         |
| sCode       | String   | 事件执行结果的code，0代表成功 |
| sMsg        | String   | 事件执行失败时的msg       |
