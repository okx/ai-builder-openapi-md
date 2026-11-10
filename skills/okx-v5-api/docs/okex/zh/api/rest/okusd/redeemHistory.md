---
site: okex
lang: zh
name: GET / 查询赎回历史
method: GET
path: /api/v5/finance/okusd/redeem/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 查询赎回历史 [financial-product-okusd-get-redemption-history]

查询您的 OKUSD 赎回历史订单。结果按时间戳倒序返回（最新优先），支持时间范围过滤及赎回类型过滤。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/okusd/redeem/history`

> 请求示例

```shell
GET /api/v5/finance/okusd/redeem/history?type=fast
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| limit | String | 否 | 每页返回条数，默认 `"100"`，最大 `"100"` |
| begin | String | 否 | 起始时间过滤（订单创建时间 `ts`，Unix ms，含） |
| end | String | 否 | 结束时间过滤（订单创建时间 `ts`，Unix ms，含） |
| type | String | 否 | 赎回类型过滤：`"fast"` 仅返回即时赎回；`"standard"` 仅返回标准赎回；不传默认返回标准赎回 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678905678",
            "clOrdId": "my-rdm-fast-001",
            "ccy": "OKUSD",
            "amt": "1000.00000000",
            "fee": "1.00000000",
            "settleCcy": "USDT",
            "settleCcyAmt": "999.00000000",
            "type": "fast",
            "status": "success",
            "estSettlementTime": "1718500010000",
            "ts": "1718500000000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| ordId | String | 系统订单 ID |
| clOrdId | String | 客户自定义订单 ID（原样返回；未传则为空字符串） |
| ccy | String | 赎回货币，固定为 `"OKUSD"` |
| amt | String | 赎回 OKUSD 数量 |
| fee | String | 实收手续费（USDT），向下截断至 8 位小数 |
| settleCcy | String | 到账货币，固定为 `"USDT"` |
| settleCcyAmt | String | 实际到账 USDT 数量（`amt - fee`，向下截断至 8 位小数） |
| type | String | 赎回类型：`"fast"`（即时赎回）或 `"standard"`（标准赎回，D+5/D+6 自然日） |
| status | String | 订单状态：`"pending"` / `"success"` / `"failed"` / `"canceled"` |
| estSettlementTime | String | 预计到账时间，Unix 时间戳，单位为毫秒。已结算的即时赎回订单返回空字符串 |
| ts | String | 订单创建时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
