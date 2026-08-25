---
site: okex
lang: zh
name: GET / 获取产品信息
method: GET
path: /api/v5/finance/staking-defi/sol/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取产品信息 [financial-product-sol-staking-get-product-info]

#### 限速：3 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/sol/product-info`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/sol/product-info

```

```python

```

> 返回结果

```json
{
    "code": "0",
    "data": {
        "fastRedemptionAvail": "240",
        "fastRedemptionDailyLimit": "240",
        "rate": "5.57",
        "redemptDays": "2",
        "minAmt": "0.01"
    },
    "msg": ""
}
```

#### 返回参数

| 参数名                           | 类型     | 描述                           |
|-------------------------------|--------|------------------------------|
| fastRedemptionDailyLimit      | String | 快速赎回每日最高份额<br>母账户和子账户共享同一个限额 |
| fastRedemptionAvail           | String | 当前剩余最大可赎回数量                  |
| rate                          | String | 最新 OKSOL 年化收益率               |
| redemptDays                   | String | OKSOL 赎回天数                   |
| minAmt                        | String | OKSOL 最低申购数量                 |
