---
site: okex
lang: zh
name: GET /查看历史持仓信息
method: GET
path: /api/v5/tradingBot/signal/positions-history
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET /查看历史持仓信息 [order-book-trading-signal-bot-trading-get-position-history]

获取最近3个月有更新的仓位信息，按照仓位更新时间倒序排列。组合保证金账户模式不支持查询历史持仓。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/positions-history`

> 请求示例

```shell
GET /api/v5/tradingBot/signal/positions-history?algoId=1234
```

#### 请求参数 

| 参数名   | 类型   | 是否必须 | 描述                                                                                    |
| :------- | :----- | :------- |:--------------------------------------------------------------------------------------|
| algoId    | String | 是       | 策略ID                                                                                      |
| instId    | String | 否       | 交易产品ID，如：`BTC-USD-SWAP`                                                                |
| after     | String | 否       | 查询仓位更新 (uTime) 之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`              |
| before    | String | 否       | 查询仓位更新 (uTime) 之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`              |
| limit     | String | 否       | 分页返回结果的数量，最大为100，默认100条                                                          |

> 返回结果

```json
{
  "code": "0",
  "data": [
    {
      "cTime": "1704724451471",
      "closeAvgPx": "200",
      "direction": "net",
      "instId": "ETH-USDT-SWAP",
      "lever": "5.0",
      "mgnMode": "cross",
      "openAvgPx": "220",
      "pnl": "-2.021",
      "pnlRatio": "-0.4593181818181818",
      "uTime": "1704724456322",
      "uly": "ETH-USDT"
    }
  ],
  "msg": ""
}
```

#### 返回参数 

| 参数名               | 类型   | 描述                                                                                    |
|:------------------| :----- |:--------------------------------------------------------------------------------------|
| instId            | String | 交易产品ID                                                                                |
| mgnMode	        | String | 保证金模式 `cross`：全仓，`isolated`：逐仓"                                                   |
| cTime             | String | 仓位创建时间                                                                                |
| uTime             | String | 仓位更新时间                                                                                |
| openAvgPx         | String | 开仓均价                                                                                  |
| closeAvgPx        | String | 平仓均价                                                                                  |
| pnl               | String | 平仓收益额                                                                                 |
| pnlRatio          | String | 平仓收益率                                                                                 |
| lever             | String | 杠杆倍数                                                                                  |
| direction         | String | 持仓方向 `long`：多 `short`：空                                                            |
| uly               | String | 标的指数                                                                                  |                                                                          |
