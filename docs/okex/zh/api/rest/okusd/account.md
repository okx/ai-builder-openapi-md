---
site: okex
lang: zh
name: GET / 查询账户余额及累计收益
method: GET
path: /api/v5/finance/okusd/account
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查询账户余额及累计收益 [financial-product-okusd-get-okusd-account]

查询您当前的 OKUSD 余额及持仓期间累计收益。所有余额均以母账户维度聚合，子账户共享。

#### 限速：2次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/okusd/account`

> 请求示例

```shell
GET /api/v5/finance/okusd/account
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
            "ccy": "OKUSD",
            "amt": "10000.00000000",
            "totalEarnAccrual": "123.45678900",
            "ts": "1718500000000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| ccy | String | 持仓货币，固定为 `"OKUSD"` |
| amt | String | 当前 OKUSD 余额 |
| totalEarnAccrual | String | 持仓期间累计收益（USDT） |
| ts | String | 服务器时间戳，Unix 时间戳，单位为毫秒，如 `1597026383085` |
