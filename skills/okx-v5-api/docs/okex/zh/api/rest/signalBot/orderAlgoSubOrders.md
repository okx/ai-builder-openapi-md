---
site: okex
lang: zh
name: GET / 获取信号策略子订单信息
method: GET
path: /api/v5/tradingBot/signal/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取信号策略子订单信息 [order-book-trading-signal-bot-trading-get-signal-bot-sub-orders]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/sub-orders`

> 请求示例

```shell
# 查询已成交历史子订单
GET /api/v5/tradingBot/signal/sub-orders?algoId=623833708424069120&algoOrdType=contract&state=filled

# 查询指定子订单
GET /api/v5/tradingBot/signal/sub-orders?algoId=623833708424069120&algoOrdType=contract&signalOrdId=O632302662327996418
```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                           |
|:------------| :----- |:-----|:-----------------------------------------------------------------------------|
| algoId      | String | 是     | 策略ID                                                                       |
| algoOrdType | String | 是     | 策略类型<br>`contract`：合约信号                                                    |
| state       | String | 可选     | 子订单状态<br>`live`：未成交<br>`partially_filled`：部分成交<br>`filled`：已成交<br>`canceled`：已取消<br>state 和 signalOrdId 必须传一个，若传两个，以 state 为主|
| signalOrdId | String | 可选     | 子订单ID                                                                   |
| after       | String | 否     | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`ordId`                                         |
| before      | String | 否     | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`ordId`                                         |
| begin       | String | 否     | 请求`cTime`在此时间戳之后(包含)的数据，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`               |
| end         | String | 否     | 请求`cTime`在此时间戳之前(包含)的数据，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`               |
| limit       | String | 否     | 返回结果的数量，最大为100，默认100条                                                        |
| type        | String | 否     | 子订单类型<br>`live`：未成交<br>`filled`：已成交<br>`即将废弃` |
| clOrdId     | String | 否     | 子订单自定义订单ID  <br>`即将废弃`                                                                 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "18",
            "algoClOrdId": "",
            "algoId": "623833708424069120",
            "algoOrdType": "contract",
            "avgPx": "1572.81",
            "cTime": "1697024702320",
            "ccy": "",
            "clOrdId": "O632302662327996418",
            "ctVal": "0.01",
            "fee": "-0.1415529",
            "feeCcy": "USDT",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "lever": "10",
            "ordId": "632302662351958016",
            "ordType": "market",
            "pnl": "-2.6784",
            "posSide": "net",
            "px": "",
            "side": "buy",
            "state": "filled",
            "sz": "18",
            "tag": "",
            "tdMode": "cross",
            "uTime": "1697024702322"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型**     | **描述**                                                                                                                   |
|:--------------|:-----------|:-------------------------------------------------------------------------------------------------------------------------|
| algoId        | String     | 策略ID                                                                                                                     |
| algoClOrdId   | String     | 用户自定义策略ID，将来扩展使用。                                                                                                        |
| instType      | String     | 产品类型                                                                                                                     |
| instId        | String     | 交易产品ID                                                                                                                   |
| algoOrdType   | String     | 策略类型<br>`contract`：合约信号                                                                                                  |
| ordId         | String     | 子订单ID                                                                                                                    |
| clOrdId         | String     | 子订单自定义ID，等同于`signalOrdId`                                                                                                                  |
| cTime         | String     | 子订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                  |
| uTime         | String     | 子订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                  |
| tdMode        | String     | 子订单交易模式<br>`cross`：全仓<br>`isolated`：逐仓<br>`cash`：非保证金                                                                    |
| ccy           | String     | 保证金币种<br>仅适用于`合约模式`下的`全仓杠杆`订单                                                                                          |
| ordType       | String     | 子订单类型<br>`market`：市价单<br>`limit`：限价单<br>`ioc`：立即成交并取消剩余                                                                  |
| sz            | String     | 子订单委托数量                                                                                                                  |
| state         | String     | 子订单状态<br>`canceled`：撤单成功<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交<br>`cancelling`：撤单中                  |
| side          | String     | 子订单订单方向<br>`buy`：买<br>`sell`：卖                                                                                           |
| px            | String     | 子订单委托价格                                                                                                                  |
| fee           | String     | 子订单手续费数量                                                                                                                 |
| feeCcy        | String     | 子订单手续费币种                                                                                                                 |
| avgPx         | String     | 子订单平均成交价格                                                                                                                |
| accFillSz     | String     | 子订单累计成交数量                                                                                                                |
| posSide       | String     | 子订单持仓方向<br>`net`：买卖模式                                                                                                    |
| pnl           | String     | 子订单收益                                                                                                                    |
| ctVal         | String     | 合约面值<br>仅支持`FUTURES/SWAP`                                                                                                |
| lever         | String     | 杠杆倍数                                                                                                                     |
| tag           | String     | 订单标签                                                                                                                     |
