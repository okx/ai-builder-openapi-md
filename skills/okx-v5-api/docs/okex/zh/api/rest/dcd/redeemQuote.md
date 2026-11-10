---
site: okex
lang: zh
name: POST / 获取赎回报价
method: POST
path: /api/v5/finance/sfp/dcd/redeem-quote
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 获取赎回报价 [financial-product-dual-investment-post-request-for-redeem-quote]

为生效中的双币赢订单申请提前赎回报价。这是两步赎回流程的第一步，之后需调用 POST / 赎回 确认。

#### 限速：10次/60s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/sfp/dcd/redeem-quote`

> 请求示例

```shell
POST /api/v5/finance/sfp/dcd/redeem-quote
body
{
    "ordId": "987654321"
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| ordId | String | 是 | 订单ID |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "quoteId": "quoterbcDCD-REDEEM17732116652401234",
            "redeemCcy": "BTC",
            "redeemSz": "1.4856",
            "termRate": "-0.50",
            "validUntil": "1774598400000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| ordId | String | 订单ID |
| quoteId | String | 报价ID |
| redeemSz | String | 赎回数量 |
| redeemCcy | String | 赎回币种 |
| termRate | String | 期限利率 |
| validUntil | String | 赎回报价有效期，Unix时间戳的毫秒数格式，如 `1597026383085` |
