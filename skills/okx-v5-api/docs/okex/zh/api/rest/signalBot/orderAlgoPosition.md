---
site: okex
lang: zh
name: GET / 获取信号策略持仓
method: GET
path: /api/v5/tradingBot/signal/positions
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取信号策略持仓 [order-book-trading-signal-bot-trading-get-signal-bot-order-positions]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/positions`

> 请求示例

```shell
GET /api/v5/tradingBot/signal/positions?algoId=623833708424069120&algoOrdType=contract
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                      |
| :-------- | :----- | :------- |:------------------------|
| algoOrdType | String | 是       | 订单类型<br>`contract`：合约信号 |
| algoId    | String | 是       | 策略ID                  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "algoClOrdId": "",
            "algoId": "623833708424069120",
            "avgPx": "1597.74",
            "cTime": "1697502301460",
            "ccy": "USDT",
            "imr": "23.76495",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "last": "1584.34",
            "lever": "10",
            "liqPx": "1438.7380360728976",
            "markPx": "1584.33",
            "mgnMode": "cross",
            "mgnRatio": "11.719278420807477",
            "mmr": "1.9011959999999997",
            "notionalUsd": "237.75168928499997",
            "pos": "15",
            "posSide": "net",
            "uTime": "1697502301460",
            "upl": "-2.0115000000000123",
            "uplRatio": "-0.0839310526118142"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                                   |
|:--------------| :------- |:-----------------------------------------|
| algoId        | String   | 策略ID                                   |
| algoClOrdId   | String   | 用户自定义策略ID，将来扩展使用。                 |
| instType      | String   | 产品类型                                     |
| instId        | String   | 产品ID，如 `BTC-USDT-SWAP`                    |
| cTime         | String   | 策略创建时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| uTime         | String   | 策略更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| avgPx         | String   | 开仓均价                                     |
| ccy           | String   | 保证金币种                                    |
| lever         | String   | 杠杆倍数                                     |
| liqPx         | String   | 预估强平价                                    |
| posSide       | String   | 持仓方向<br>`net`：买卖模式                       |
| pos           | String   | 持仓数量                                     |
| mgnMode       | String   | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓     |
| mgnRatio      | String   | 维持保证金率                                     |
| imr           | String   | 初始保证金                                    |
| mmr           | String   | 维持保证金                                    |
| upl           | String   | 未实现收益                                    |
| uplRatio      | String   | 未实现收益率                                   |
| last          | String   | 最新成交价                                    |
| notionalUsd   | String   | 仓位美金价值                                   |
| adl           | String   | 自动减仓信号区<br>分为5档，从1到5，数字越小代表adl强度越弱           |
| markPx        | String   | 标记价格                                     |
