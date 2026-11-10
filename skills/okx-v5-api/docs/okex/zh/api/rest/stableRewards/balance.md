---
site: okex
lang: zh
name: GET / 获取余额
method: GET
path: /api/v5/finance/stable-rewards/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取余额 [financial-product-stable-rewards-get-balance]

查询 Stable Rewards 的实时余额，余额涵盖交易账户、资金账户以及正在赎回中的资产合计，同时返回累计收益与当前收益状态。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/stable-rewards/balance`

> 请求示例

```shell
GET /api/v5/finance/stable-rewards/balance?ccy=USDG
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| ccy | String | 否 | 稳定币，如 `USDG`<br>不传则返回全部支持的稳定币 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "details": [
                {
                    "ccy": "USDG",
                    "amt": "100",
                    "totalEarnAccrual": "0.0003",
                    "state": "earning"
                }
            ],
            "ts": "1718035200000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| details | Array of objects | 按稳定币返回的实时余额明细 |
| > ccy | String | 稳定币，如 `USDG` |
| > amt | String | 整个账户范围内的持有数量 |
| > totalEarnAccrual | String | 持有期间的累计收益 |
| > state | String | 收益状态<br>`earning`：正在产生收益<br>`pending`：未在产生收益（如自动赚币已关闭，或余额低于起息门槛） |
| ts | String | 数据查询时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
