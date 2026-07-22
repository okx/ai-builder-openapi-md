---
site: okex
lang: zh
name: GET / 获取历史订单
method: GET
path: /api/v5/finance/sfp/dcd/order-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取历史订单 [financial-product-dual-investment-get-order-history]

返回双币赢历史订单列表

#### 限速：1次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/sfp/dcd/order-history`

> 请求示例

```shell
GET /api/v5/finance/sfp/dcd/order-history
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| ordId | String | 否 | 订单ID。传入时直接返回该订单（忽略其他筛选条件） |
| productId | String | 否 | 产品ID，如 `BTC-USDT-260327-77000-C` |
| uly | String | 否 | 标的指数，如 `BTC-USD` |
| state | String | 否 | 订单状态筛选<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
| beginId | String | 否 | 返回比该订单ID更新的记录 |
| endId | String | 否 | 返回比该订单ID更早的记录 |
| begin | String | 否 | 开始时间戳筛选，Unix时间戳的毫秒数格式，如 `1597026383085` |
| end | String | 否 | 结束时间戳筛选，Unix时间戳的毫秒数格式，如 `1597026383085` |
| limit | String | 否 | 每次请求返回的结果数量，最大100 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "quoteId": "quoterbpDCD-QUOTE17732116652401234",
            "state": "settled",
            "productId": "BTC-USDT-260327-77000-C",
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "uly": "BTC-USD",
            "strike": "77000",
            "notionalSz": "1.5",
            "notionalCcy": "BTC",
            "absYield": "0.00806038",
            "annualizedYield": "0.1834",
            "yieldSz": "0.01209057",
            "yieldCcy": "BTC",
            "settleSz": "1.51209057",
            "settleCcy": "BTC",
            "settlePx": "76500",
            "settleTime": "1774598400000",
            "expTime": "1774598400000",
            "redeemStartTime" : "1774598400000",
            "redeemEndime": "1774598400000",
            "cTime": "1773212400000",
            "uTime": "1773212400000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| ordId | String | 订单ID |
| quoteId | String | 报价ID |
| state | String | 订单状态<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
| productId | String | 产品ID，如 `BTC-USDT-260327-77000-C` |
| baseCcy | String | 基础币种，如 `BTC` |
| quoteCcy | String | 计价币种，如 `USDT` |
| uly | String | 标的指数，如 `BTC-USD` |
| strike | String | 行权价 |
| notionalSz | String | 投资数量 |
| notionalCcy | String | 投资币种 |
| absYield | String | 绝对收益率 |
| annualizedYield | String | 年化收益率 |
| yieldSz | String | 收益金额 |
| yieldCcy | String | 收益币种 |
| settleSz | String | 结算金额（未结算时为""） |
| settleCcy | String | 结算币种（未结算时为""） |
| settlePx | String | 结算价格（未结算时为""） |
| expTime | String | 产品到期时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| settleTime | String | 实际结算时间，Unix时间戳的毫秒数格式，如 `1597026383085`（未结算时为""） |
| redeemStartTime | String | 最早可申请提前赎回的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| redeemEndTime | String | 最晚可申请提前赎回的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| cTime | String | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| uTime | String | 最后更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
