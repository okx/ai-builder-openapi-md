---
site: okex
lang: zh
name: POST / 申购 OKUSD
method: POST
path: /api/v5/finance/okusd/subscribe
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 申购 OKUSD [financial-product-okusd-subscribe-okusd]

以 1:1 汇率将 USDT 申购为 OKUSD，无申购手续费，OKUSD 即时到账至资金账户。每次请求需传入唯一的 `clOrdId`；重复提交相同 `clOrdId` 将直接返回原始订单，不重复执行申购。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/okusd/subscribe`

> 请求示例

```shell
POST /api/v5/finance/okusd/subscribe
body
{
    "amt": "1000.00000000",
    "clOrdId": "my-sub-001"
}
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| amt | String | 是 | 申购 USDT 数量。最小值：`1`。最多 8 位小数。不支持科学计数法 |
| clOrdId | String | 是 | 客户自定义订单 ID，最多 32 字符（字母、数字、`-`、`_`）。同一 UID 下不可重复，用于订单追踪与幂等标识 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678901234",
            "clOrdId": "my-sub-001",
            "ccy": "USDT",
            "amt": "1000.00000000",
            "okusdAmt": "1000.00000000",
            "state": "success",
            "ts": "1718500000000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| ordId | String | 系统订单 ID |
| clOrdId | String | 客户自定义订单 ID（原样返回） |
| ccy | String | 申购货币，固定为 `"USDT"` |
| amt | String | 实际申购 USDT 数量 |
| okusdAmt | String | 到账 OKUSD 数量（= `amt`，汇率 1:1，无申购手续费），到账至资金账户 |
| state | String | 订单状态：`"success"` / `"pending"` / `"failed"` |
| ts | String | 订单创建时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
