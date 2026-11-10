---
site: okex
lang: zh
name: GET / 获取交易员带单情况
method: GET
path: /api/v5/copytrading/public-stats
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取交易员带单情况 [order-book-trading-copy-trading-get-lead-trader-stats]

公共接口，获取交易员带单情况。

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-stats`

> 请求示例

```shell
GET /api/v5/copytrading/public-stats?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD&lastDays=1

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
            "avgSubPosNotional": "213.1038",
            "ccy": "USDT",
            "curCopyTraderPnl": "96.8071",
            "investAmt": "265.095252476476294",
            "lossDays": "1",
            "profitDays": "2",
            "winRatio": "0.6667"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**            | **类型** | **描述**                             |
|:-------------------|:-------|:-----------------------------------|
| winRatio            | String | 胜率                         |
| profitDays          | String | 盈利天数                              |
| lossDays              | String  | 亏损天数                              |
| curCopyTraderPnl              | String  | 当前跟随者收益 (USDT)                              |
| avgSubPosNotional              | String  | 平均仓位价值 (USDT)                              |
| investAmt              | String  | 带单本金 (USDT)                             |
| ccy              | String  | 保证金币种                            |
