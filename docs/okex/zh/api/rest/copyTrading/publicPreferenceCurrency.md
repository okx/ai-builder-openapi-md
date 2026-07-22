---
site: okex
lang: zh
name: GET / 获取交易员币种偏好
method: GET
path: /api/v5/copytrading/public-preference-currency
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取交易员币种偏好 [order-book-trading-copy-trading-get-lead-trader-currency-preferences]

公共接口，获取交易员币种偏好，返回结果按 ratio 从大到小排序

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-preference-currency`

> 请求示例

```shell
GET /api/v5/copytrading/public-preference-currency?instType=SWAP&uniqueCode=CB4594A3BB5D3538

```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                      |
|:------------| :----- |:-----|:------------------------------------------------------------------------|
| instType    | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位）                       |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "ETH",
            "ratio": "0.8881"
        },
        {
            "ccy": "BTC",
            "ratio": "0.0666"
        },
        {
            "ccy": "YFII",
            "ratio": "0.0453"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**            | **类型** | **描述**        |
|:-------------------|:-------|:--------------|
| ccy            | String | 币种            |
| ratio          | String | 占比，0.1 代表 10% |
