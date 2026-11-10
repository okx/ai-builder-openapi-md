---
site: my
lang: zh
name: 获取市场借币杠杆利率和借币限额（私有）
method: GET
path: /api/v5/account/interest-rate-loan-quota
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取市场借币杠杆利率和借币限额（私有） [public-data-rest-api-get-interest-rate-and-loan-quota-private]

这是一个私有接口，用户需要通过鉴权才能访问。

获取用户所在站点的利率及借币限额。

#### 限速：2次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/interest-rate-loan-quota`

> 请求示例

```shell
GET /api/v5/account/interest-rate-loan-quota
```

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "configCcyList": [
                {
                    "ccy": "USDT",
                    "rate": "0.00043728",
                }
            ],
            "basic": [
                {
                    "ccy": "USDT",
                    "quota": "500000",
                    "rate": "0.00043728"
                },
                {
                    "ccy": "BTC",
                    "quota": "10",
                    "rate": "0.00019992"
                }
            ],
            "vip": [
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "6",
                    "level": "VIP1"
                },
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "7",
                    "level": "VIP2"
                }
            ],
            "config": [
                {
                    "ccy": "USDT",
                    "stgyType": "0",    // normal
                    "quota": "xxxxxx",
                    "level": "VIP 8"
                },
                ......
                {
                    "ccy": "USDT",
                    "stgyType": "1",    // delta neutral
                    "quota": "xxxxx",
                    "level": "VIP 1"
                },
                ......
            ],
            "regular": [
                {
                    "irDiscount": "",
                    "loanQuotaCoef": "1",
                    "level": "Lv1"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型   | 描述                         |
| :----- | :----- |:---------------------------|
| basic      | Array of objects | 基础利率和借币限额                  |
| > ccy      | String | 币种                         |
| > rate     | String | 日借币利率                      |
| > quota    | String | 基础借币限额                     |
| vip        | Array of objects | 专业用户                       |
| > level      | String | 账户交易手续费等级，如 `VIP1`         |
| > loanQuotaCoef     | String | 借币限额系数，借币限额 = 基础借币限额 * 该系数 |
| > irDiscount    | String | <del>利率的折扣率</del>(已废弃)     |
| regular      | Array of objects | 普通用户                       |
| > level      | String | 账户交易手续费等级，如 `Lv1`          |
| > loanQuotaCoef     | String | 借币限额系数，借币限额 = 基础借币限额 * 该系数                    |
| > irDiscount    | String | <del>利率的折扣率</del>(已废弃)     |
| configCcyList | Array of strings | 由自定义绝对值方式配置借币限额的币种<br>当币种在configCcyList中时，用户应该参考config以获取相应限额，而非使用basic/vip/regular |
| > ccy | String | 币种 |
| > rate | String | 基础杠杆日利率 |
| config | Array of objects | 由自定义绝对值方式配置借币限额的币种详情 |
| > ccy | String | 币种 |
| > stgyType | String | 策略类型<br>`0`：普通策略模式<br>`1`：delta 中性策略模式<br>如果某个币种仅返回0，则表示该借贷额度由普通策略模式的账户和 delta 中性策略模式的账户共享；如果某个币种同时返回0/1，则表示 delta 中性策略模式的账户拥有单独的借贷额度。 |
| > quota | String | 借币限额 |
| > level | String | 账户交易手续费等级，如 `VIP1` |
