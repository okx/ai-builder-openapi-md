---
site: okex
lang: zh
name: GET / 获取币对
method: GET
path: /api/v5/finance/sfp/dcd/currency-pair
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

## 双币赢 [financial-product-dual-investment]
### GET / 获取币对 [financial-product-dual-investment-get-currency-pairs]

获取双币赢币对

#### 限速：1次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/sfp/dcd/currency-pair`

> 请求示例

```shell
GET /api/v5/finance/sfp/dcd/currency-pair
```

#### 请求参数

无

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "optType": "C",
            "uly": "BTC-USD"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| baseCcy | String | 基础币种 |
| quoteCcy | String | 报价币种 |
| optType | String | 期权类型<br>`C`：看涨<br>`P`：看跌 |
| uly | String | 标的 |
