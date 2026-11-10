---
site: okex
lang: zh
name: GET / 获取网格策略委托持仓
method: GET
path: /api/v5/tradingBot/grid/positions
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取网格策略委托持仓 [order-book-trading-grid-trading-get-grid-algo-order-positions]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/grid/positions`

> 请求示例

```shell
GET /api/v5/tradingBot/grid/positions?algoId=448965992920907776&algoOrdType=contract_grid
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoOrdType | String | 是       | 订单类型<br>`contract_grid`：合约网格委托  |
| algoId    | String | 是       | 策略订单ID                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "algoClOrdId": "",
            "algoId": "449327675342323712",
            "avgPx": "29215.0142857142857149",
            "cTime": "1653400065917",
            "ccy": "USDT",
            "imr": "2045.386",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "last": "29206.7",
            "lever": "5",
            "liqPx": "661.1684795867162",
            "markPx": "29213.9",
            "mgnMode": "cross",
            "mgnRatio": "217.19370606167573",
            "mmr": "40.907720000000005",
            "notionalUsd": "10216.70307",
            "pos": "35",
            "posSide": "net",
            "uTime": "1653400066938",
            "upl": "1.674999999999818",
            "uplRatio": "0.0008190504784478"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                                   |
|:--------------| :------- |:-----------------------------------------|
| algoId        | String   | 策略订单ID                                   |
| algoClOrdId   | String   | 用户自定义策略ID                                |
| instType      | String   | 产品类型                                     |
| instId        | String   | 产品ID，如 `BTC-USDT-SWAP`                    |
| cTime         | String   | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| uTime         | String   | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
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
