---
site: okex
lang: zh
name: GET / 获取定投策略子订单信息
method: GET
path: /api/v5/tradingBot/recurring/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取定投策略子订单信息 [order-book-trading-recurring-buy-get-recurring-buy-sub-orders]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/recurring/sub-orders`

> 请求示例

```shell
GET /api/v5/tradingBot/recurring/sub-orders?algoId=560516615079727104
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                   |
| :-------- | :----- | :------- |:-------------------------------------|
| algoId    | String | 是       | 策略订单ID                               |
| ordId    | String | 否       | 子订单ID                                |
| after    | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`ordId` |
| before    | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`ordId` |
| limit    | String | 否       | 返回结果的数量，最大为300，默认300条                |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.045315",
            "algoClOrdId": "",
            "algoId": "560516615079727104",
            "algoOrdType": "recurring",
            "avgPx": "1765.4",
            "cTime": "1679911222200",
            "fee": "-0.0000317205",
            "feeCcy": "ETH",
            "instId": "ETH-USDC",
            "instType": "SPOT",
            "ordId": "560523524230717440",
            "ordType": "market",
            "px": "-1",
            "side": "buy",
            "state": "filled",
            "sz": "80",
            "tag": "",
            "tdMode": "",
            "uTime": "1679911222207"
        },
        {
            "accFillSz": "0.00071526",
            "algoClOrdId": "",
            "algoId": "560516615079727104",
            "algoOrdType": "recurring",
            "avgPx": "27961.6",
            "cTime": "1679911222189",
            "fee": "-0.000000500682",
            "feeCcy": "BTC",
            "instId": "BTC-USDC",
            "instType": "SPOT",
            "ordId": "560523524184580096",
            "ordType": "market",
            "px": "-1",
            "side": "buy",
            "state": "filled",
            "sz": "20",
            "tag": "",
            "tdMode": "",
            "uTime": "1679911222194"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                                                  |
| :--------- | :------- |:--------------------------------------------------------------------------------------------------------|
| algoId      | String   | 策略订单ID                                                                                                  |
| instType      | String   | 产品类型                                                                                                    |
| instId       | String   | 产品ID                                                                                                    |
| algoOrdType       | String   | 策略订单类型<br>`recurring`：定投                                                                                |
| ordId       | String   | 子订单ID                                                                                                   |
| cTime       | String   | 子订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                 |
| uTime       | String   | 子订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                 |
| tdMode       | String   | 子订单交易模式<br>`cross`：全仓 `cash`：非保证金                                                                       |
| ordType       | String   | 子订单类型<br>`market`：市价单<br>`manual_add_order`：手动加仓单                                                                                    |
| sz       | String   | 子订单委托数量                                                                                                 |
| state       | String   | 子订单状态<br>`canceled`：撤单成功<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交<br>`cancelling`：撤单中 |
| side       | String   | 子订单订单方向<br>`buy`：买 `sell`：卖                                                                             |
| px       | String   | 子订单委托价格<br>市价委托时为"-1"                                                                                   |
| fee       | String   | 子订单手续费数量                                                                                                |
| feeCcy       | String   | 子订单手续费币种                                                                                                |
| avgPx       | String   | 子订单平均成交价格                                                                                               |
| accFillSz       | String   | 子订单累计成交数量                                                                                               |
| tag | String | 订单标签                                                                                                    |
| algoClOrdId         | String   | 用户自定义策略ID                        |
