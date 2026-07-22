---
site: okex
lang: zh
name: GET / 获取交易员收益日表现
method: GET
path: /api/v5/copytrading/public-pnl
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取交易员收益日表现 [order-book-trading-copy-trading-get-lead-trader-daily-pnl]

公共接口，获取交易员每日的收益表现，按时间倒序返回

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-pnl`

> 请求示例

```shell
GET /api/v5/copytrading/public-pnl?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD&lastDays=1

```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                      |
|:------------| :----- |:-----|:------------------------------------------------------------------------|
| instType    | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位）                       |
| lastDays  | String | 是    | 最近天数<br>`1`: 近 7 天 <br>`2`: 近 30 天<br>`3`: 近 90 天， <br>`4`: 近 365 天 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "beginTs": "1701100800000",
            "pnl": "97.3309",
            "pnlRatio": "0.3672"
        },
        {
            "beginTs": "1701014400000",
            "pnl": "96.7755",
            "pnlRatio": "0.3651"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**            | **类型** | **描述**     |
|:-------------------|:-------|:-----------|
| beginTs            | String | 当天开始时间 |
| pnl          | String | 累计收益额      |
| pnlRatio              | String  | 累计收益率      |
