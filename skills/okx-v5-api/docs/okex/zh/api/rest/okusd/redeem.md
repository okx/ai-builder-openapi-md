---
site: okex
lang: zh
name: POST / 赎回 OKUSD
method: POST
path: /api/v5/finance/okusd/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 赎回 OKUSD [financial-product-okusd-redeem-okusd]

将 OKUSD 赎回为 USDT。支持即时赎回（实时到账）和标准赎回（D+5 或 D+6 自然日，取决于提交时间）。各类型手续费率请调用 `GET /limits` 查询。所有手续费均向下截断（floor）至 8 位小数。每次请求需传入唯一的 `clOrdId`；重复提交相同 `clOrdId` 将直接返回原始订单，不重复执行赎回。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/okusd/redeem`

> 请求示例（即时赎回）

```shell
POST /api/v5/finance/okusd/redeem
body
{
    "amt": "1000.00000000",
    "redeemType": "1",
    "clOrdId": "my-redeem-001"
}
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| amt | String | 是 | 赎回 OKUSD 数量。最小值：`1`。最多 8 位小数。不支持科学计数法 |
| redeemType | String | 是 | 赎回类型。`"1"`：即时赎回（实时到账）；`"2"`：标准赎回（UTC+8 16:00 前提交加 5 自然日，16:00 后（含）提交加 6 自然日）。各类型手续费率请参考 `/limits` 接口返回的 `feeRate` 字段 |
| clOrdId | String | 是 | 客户自定义订单 ID，最多 32 字符（字母、数字、`-`、`_`）。同一 UID 下不可重复，用于订单追踪与幂等标识 |

> 返回结果（即时赎回）

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678905678",
            "clOrdId": "my-redeem-001",
            "ccy": "OKUSD",
            "amt": "1000.00000000",
            "fee": "1.00000000",
            "usdtAmt": "999.00000000",
            "redeemType": "1",
            "state": "success",
            "estSettlementTime": "1718500010000",
            "ts": "1718500000000"
        }
    ]
}
```

> 返回结果（标准赎回）

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678906789",
            "clOrdId": "my-redeem-002",
            "ccy": "OKUSD",
            "amt": "50000.00000000",
            "fee": "12.50000000",
            "usdtAmt": "49987.50000000",
            "redeemType": "2",
            "state": "processing",
            "estSettlementTime": "1718932800000",
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
| ccy | String | 赎回货币，固定为 `"OKUSD"` |
| amt | String | 赎回 OKUSD 数量 |
| fee | String | 实收手续费（USDT），向下截断至 8 位小数 |
| usdtAmt | String | 实际到账 USDT 数量（`amt - fee`，向下截断至 8 位小数），到账至资金账户 |
| redeemType | String | 赎回类型：`"1"`（即时）或 `"2"`（标准） |
| state | String | 订单状态：`"processing"` / `"success"` / `"failed"` / `"cancelled"` |
| estSettlementTime | String | 预计到账时间，Unix 时间戳，单位为毫秒。即时赎回为当前时间；标准赎回：UTC+8 16:00 前提交加 5 自然日，16:00 后（含）提交加 6 自然日 |
| ts | String | 订单创建时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
