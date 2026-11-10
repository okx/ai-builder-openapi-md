---
site: okex
lang: zh
name: POST / 获取报价
method: POST
path: /api/v5/finance/sfp/dcd/quote
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 获取报价 [financial-product-dual-investment-post-request-for-quote]

为双币赢产品请求实时报价。报价有有效期，须在到期前使用。

#### 限速：10次/60s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/sfp/dcd/quote`

> 请求示例

```shell
POST /api/v5/finance/sfp/dcd/quote
body
{
    "productId": "BTC-USDT-260327-77000-C",
    "notionalSz": "1.5",
    "notionalCcy": "BTC"
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| productId | String | 是 | 产品ID |
| notionalSz | String | 是 | 投资数量 |
| notionalCcy | String | 是 | 投资币种 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "absYield": "0.00135182",
            "annualizedYield": "69.65",
            "interestAccrualTime": "1773241200000",
            "notionalSz": "0.001",
            "notionalCcy": "BTC",
            "productId": "BTC-USDT-260312-72000-C",
            "quoteId": "qtbcDCD-QUOTE17732395560537636",
            "validUntil": "1774584000000",
            "idxPx": "69000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| absYield | String | 绝对收益率 |
| annualizedYield | String | 年化收益率|
| interestAccrualTime | String | 利息开始计算时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| notionalSz | String | 投资数量 |
| notionalCcy | String | 投资币种 |
| productId | String | 产品ID |
| quoteId | String | 报价ID |
| validUntil | String | 报价有效期，Unix时间戳的毫秒数格式，如 `1597026383085` |
| idxPx | String | 指数价格 |
