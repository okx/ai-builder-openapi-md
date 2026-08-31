---
site: us
lang: zh
name: 调整模拟盘余额
method: POST
path: /api/v5/account/demo-adjust-balance
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 调整模拟盘余额 [trading-account-rest-api-demo-adjust-balance]

**此接口仅适用于模拟交易环境。**

允许用户对模拟账户中特定币种（BTC、ETH、USDT、OKB）的余额进行增加或减少，以便在不同资金情况下灵活测试交易策略。

原子性操作：若请求中任意币种未通过业务校验，整个请求将被拒绝，所有币种余额均不做修改。

#### 限速：增加 — 每用户每天 3 次（UTC 0:00 重置）；减少 — 无限制
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/demo-adjust-balance`

> 请求示例

```shell
POST /api/v5/account/demo-adjust-balance
body
{
    "type": "increase",
    "adjustments": [
        { "ccy": "BTC", "amt": "0.5" },
        { "ccy": "USDT", "amt": "3000" }
    ]
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| type | String | 是 | 调整方向。<br>`increase`：增加余额<br>`reduce`：减少余额<br>每次请求只能选择一个方向，不可同时包含增加和减少。 |
| adjustments | Array | 是 | 币种调整列表，至少包含一项，不允许重复币种。 |
| > ccy | String | 是 | 币种。支持：`BTC` `ETH` `USDT` `OKB` |
| > amt | String | 是 | 调整数量。必须为非负数，小数位数不超过该币种精度。<br>单次增加上限：BTC：1，ETH：1，USDT：5000，OKB：100。<br>减少操作无单次数量限制，仅受可用余额 ≥ 0 约束。 |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "remainCnt": "2",
        "totalCnt": "3",
        "details": [
            { "ccy": "BTC", "amt": "0.5", "bal": "1.5" },
            { "ccy": "USDT", "amt": "3000", "bal": "13000" }
        ]
    }]
}
```

> 失败示例

```json
{
    "code": "59693",
    "msg": "USDT transferable balance insufficient. Some funds are occupied by open orders or positions. Please cancel orders or close positions and try again",
    "data": []
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| remainCnt | String | 当日剩余增加余额次数。减少操作也会返回该字段，但减少操作不消耗次数。 |
| totalCnt | String | 每日增加余额总次数（默认为 3）。 |
| details | Array | 各币种操作详情。 |
| > ccy | String | 币种。 |
| > amt | String | 实际调整数量。 |
| > bal | String | 操作后该币种的余额。 |
