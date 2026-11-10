---
site: okex
lang: zh
name: GET / 获取期权公共成交数据
method: GET
path: /api/v5/public/option-trades
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取期权公共成交数据[order-book-trading-market-data-get-option-trades]

最多返回最近的100条成交数据

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/option-trades`

> 请求示例

```shell
GET /api/v5/public/option-trades?instFamily=BTC-USD
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                                         |
|:---------------| :----- |:-----|:---------------------------------------------------------------------------------------------------------------------------|
| instId       | String | 可选   | 产品ID，如 BTC-USD-221230-4000-C，`instId` 和 `instFamily` 必须传一个，若传两个，以 `instId` 为主                                                 |
| instFamily         | String | 可选   | 交易品种，如 BTC-USD                                                                                         |
| optType            | String | 否    | 期权类型，`C`：看涨期权 `P`：看跌期权 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "fillVol": "0.24415013671875",
            "fwdPx": "16676.907614127158",
            "idxPx": "16667",
            "instFamily": "BTC-USD",
            "instId": "BTC-USD-221230-16600-P",
            "markPx": "0.006308943261227884",
            "optType": "P",
            "px": "0.005",
            "side": "sell",
            "sz": "30",
            "tradeId": "65",
            "ts": "1672225112048"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                               |
| :--------- | :------- |:-------------------------------------|
| instId     | String   | 产品ID                                 |
| instFamily     | String   | 交易品种                                 |
| tradeId    | String   | 成交ID                                 |
| px         | String   | 成交价格                                 |
| sz         | String   | 成交数量。单位为张。                           |
| side       | String   | 成交方向 <br>`buy`：买  <br>`sell`：卖       |
| optType       | String   | 期权类型，C：看涨期权 P：看跌期权 ，仅适用于期权           |
| fillVol       | String   | 成交时的隐含波动率（对应成交价格）                    |
| fwdPx       | String   | 成交时的远期价格                             |
| idxPx       | String   | 成交时的指数价格                             |
| markPx       | String   | 成交时的标记价格                             |
| ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如`1597026383085` |
