---
site: okex
lang: zh
name: GET / 获取一键还债历史记录(新)
method: GET
path: /api/v5/trade/one-click-repay-history-v2
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取一键还债历史记录(新) [order-book-trading-trade-get-one-click-repay-history-new]

查询一键还债近7天的历史记录与进度状态。仅适用于`现货模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/one-click-repay-history-v2`

> 请求示例

```shell
GET /api/v5/trade/one-click-repay-history-v2
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)
result = tradeAPI.oneclick_repay_history_v2()
print(result)
```

#### 请求参数

| 参数名    | 类型    | 是否必须  | 描述                                                           |
| :------- | :----- | :------- |:-------------------------------------------------------------|
| after    | String | 否       | 查询在指定请求时间`ts`之前(包含)的内容，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
| before   | String | 否       | 查询在指定请求时间`ts`之后(包含)的内容，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
| limit    | String | 否       | 返回的结果集数量，默认为`100`，最大为`100`                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "fillDebtSz": "9.079631989",
            "ordIdInfo": [
                {
                    "cTime": "1742194485439",
                    "fillPx": "1",
                    "fillSz": "9.088651",
                    "instId": "USDC-USDT",
                    "ordId": "2338478342062235648",
                    "ordType": "ioc",
                    "px": "1.0049",
                    "side": "buy",
                    "state": "filled",
                    "sz": "9.0886514537313433"
                },
                {
                    "cTime": "1742194482326",
                    "fillPx": "83271.9",
                    "fillSz": "0.00010969",
                    "instId": "BTC-USDT",
                    "ordId": "2338478237607288832",
                    "ordType": "ioc",
                    "px": "82856.7",
                    "side": "sell",
                    "state": "filled",
                    "sz": "0.000109696512171"
                }
            ],
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "status": "filled",
            "ts": "1742194481852"
        },
        {
            "debtCcy": "USDC",
            "fillDebtSz": "100",
            "ordIdInfo": [],
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "status": "filled",
            "ts": "1742192217511"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**      | **类型**            | **描述**                                                      |
|:-------------|:------------------|:------------------------------------------------------------|
| debtCcy      | String            | 负债币种                                                        |
| repayCcyList | Array of strings            | 偿还币种列表，如 ["USDC","BTC"]                                     |
| fillDebtSz   | String            | 对应的负债币种成交数量                                                 |
| status       | String            | 当前还债进度/状态<br>`running`：进行中 <br>`filled`：已完成 <br>`failed`：失败 |
| ordIdInfo    | Array of objects   | 相关订单信息                                                      |
| > ordId      | String            | 订单ID                                                        |
| > instId     | String            | 产品ID，如 `BTC-USDT`                                           |
| > ordType    | String            | 订单类型<br>`ioc`：立即成交并取消剩余                                     |
| > side       | String            | 订单方向<br>`buy`<br>`sell`                                     |
| > px         | String            | 委托价格                                                        |
| > sz         | String            | 委托数量                                                        |
| > fillPx     | String            | 最新成交价格<br>如果成交数量为0，该字段为""                                   |
| > fillSz     | String            | 最新成交数量                                                      |
| > state      | String            | 订单状态<br>`filled`：完全成交<br>`canceled`：撤单成功                    |
| > cTime      | String            | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                      |
| ts           | String            | 请求时间，Unix时间戳的毫秒数格式，如 `1597026383085`                        |
