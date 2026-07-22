---
site: okex
lang: zh
name: GET / 查询申购历史
method: GET
path: /api/v5/finance/okusd/subscribe/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查询申购历史 [financial-product-okusd-get-subscription-history]

查询您的 OKUSD 申购历史订单。结果按时间戳倒序返回（最新优先），支持时间范围过滤。

#### 限速：5次/2s
#### 限速规则：User ID
#### Permission: Read

#### HTTP 请求

`GET /api/v5/finance/okusd/subscribe/history`

> 请求示例

```shell
GET /api/v5/finance/okusd/subscribe/history?limit=2
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| limit | String | 否 | 每页返回条数，默认 `"100"`，最大 `"100"` |
| begin | String | 否 | 起始时间过滤（订单创建时间 `ts`，Unix ms，含） |
| end | String | 否 | 结束时间过滤（订单创建时间 `ts`，Unix ms，含） |

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
            "settleCcy": "OKUSD",
            "settleCcyAmt": "1000.00000000",
            "status": "success",
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
| ccy | String | 申购货币，固定为 `"USDT"` |
| amt | String | 申购 USDT 数量 |
| settleCcy | String | 到账货币，固定为 `"OKUSD"` |
| settleCcyAmt | String | 到账 OKUSD 数量（= `amt`，汇率 1:1） |
| status | String | 订单终态：`"success"` / `"failed"` |
| ts | String | 订单创建时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
