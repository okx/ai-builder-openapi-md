---
site: okex
lang: zh
name: GET / 获取期权品种公共成交数据
method: GET
path: /api/v5/market/option/instrument-family-trades
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取期权品种公共成交数据 [order-book-trading-market-data-get-option-trades-by-instrument-family]

查询期权同一个交易品种下的成交信息数据，最多返回100条。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/option/instrument-family-trades`

> 请求示例

```shell
GET /api/v5/market/option/instrument-family-trades?instFamily=BTC-USD
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instFamily | String | 是       | 交易品种，如 BTC-USD，适用于期权                      |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "vol24h": "103381",
            "tradeInfo": [
                {
                    "instId": "BTC-USD-221111-17750-C",
                    "side": "sell",
                    "sz": "1",
                    "px": "0.0075",
                    "tradeId": "20",
                    "ts": "1668090715058"
                },
                {
                    "instId": "BTC-USD-221111-17750-C",
                    "side": "sell",
                    "sz": "91",
                    "px": "0.01",
                    "tradeId": "19",
                    "ts": "1668090421062"
                }
            ],
            "optType": "C"
        },
        {
            "vol24h": "144499",
            "tradeInfo": [
                {
                    "instId": "BTC-USD-230127-10000-P",
                    "side": "sell",
                    "sz": "82",
                    "px": "0.019",
                    "tradeId": "23",
                    "ts": "1668090967057"
                },
                {
                    "instId": "BTC-USD-221111-16250-P",
                    "side": "sell",
                    "sz": "102",
                    "px": "0.0045",
                    "tradeId": "24",
                    "ts": "1668090885050"
                }
            ],
            "optType": "P"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                             |
| :--------- | :------- |:-----------------------------------|
| vol24h     | String   | 24小时成交量，以张为单位                      |
| optType    | String   | 期权类型，`C`：看涨期权 `P`：看跌期权             |
| tradeInfo         | Array of objects   | 成交数据列表                             |
| > instId         | String   | 产品ID                               |
| > tradeId       | String   | 成交ID                               |
| > px         | String   | 成交价格                               |
| > sz         | String   | 成交数量，单位为张。                         |
| > side         | String   | 成交方向<br>`buy`：买<br>`sell`：卖        |
| > ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如1597026383085 |
