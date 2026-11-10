---
site: okex
lang: zh
name: GET / 获取产品信息
method: GET
path: /api/v5/finance/sfp/dcd/products
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取产品信息 [financial-product-dual-investment-get-product-info]

获取双币赢产品列表

#### 限速：1次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/sfp/dcd/products`

> 请求示例

```shell
GET /api/v5/finance/sfp/dcd/products?baseCcy=BTC&quoteCcy=USDT&optType=C
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| baseCcy | String | 是 | 基础币种 |
| quoteCcy | String | 是 | 报价币种 |
| optType | String | 是 | 期权类型<br>`C`：看涨<br>`P`：看跌 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "absYield": "0.00232413",
            "annualizedYield": "0.0541",
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "expTime": "1774598400000",
            "interestAccrualTime": "1773244800000",
            "listTime": "1743150759000",
            "maxSize": "6000000",
            "minSize": "10",
            "notionalCcy": "USDT",
            "optType": "P",
            "productId": "BTC-USDT-260327-54500-P",
            "quoteTime": "1773243808703",
            "redeemEndTime": "1774594800000",
            "redeemStartTime": "1773244800000",
            "stepSz": "1",
            "tradeEndTime": "1774584000000",
            "strike": "54500",
            "uly": "BTC-USD"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| absYield | String | 绝对收益率 |
| annualizedYield | String | 年化收益率 |
| baseCcy | String | 基础币种 |
| quoteCcy | String | 报价币种 |
| notionalCcy | String | 投资币种。若 `C`，则为 baseCcy；若 `P`，则为 quoteCcy。 |
| expTime | String | 到期时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| interestAccrualTime | String | 利息开始计算时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| listTime | String | 产品上架时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| minSize | String | 最小交易规模（以投资币种计） |
| maxSize | String | 最大交易规模（以投资币种计） |
| optType | String | 期权类型<br>`C`：看涨<br>`P`：看跌 |
| productId | String | 产品ID |
| quoteTime | String | 产品报价时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| redeemStartTime | String | 最早可申请提前赎回的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| redeemEndTime | String | 最晚可申请提前赎回的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| stepSz | String | 交易步长（以投资币种计） |
| tradeEndTime | String | 交易截止时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| uly | String | 标的 |
| strike | String | 行权价 |
