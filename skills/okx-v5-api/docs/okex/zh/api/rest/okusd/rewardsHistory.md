---
site: okex
lang: zh
name: GET / 查询收益发放历史
method: GET
path: /api/v5/finance/okusd/rewards/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查询收益发放历史 [financial-product-okusd-get-rewards-history]

查询您的 OKUSD 每日收益发放历史。结果按时间戳倒序返回（最新优先）。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/okusd/rewards/history`

> 请求示例

```shell
GET /api/v5/finance/okusd/rewards/history?limit=7
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| limit | String | 否 | 每页返回条数，默认 `"30"`，最大 `"100"` |
| begin | String | 否 | 起始时间过滤（`ts`，Unix ms，含），最大查询跨度 6 个月 |
| end | String | 否 | 结束时间过滤（`ts`，Unix ms，含），不传默认取当前时间 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ccy": "USDT",
            "earnAmt": "1.14246575",
            "amt": "10000.00000000",
            "apr": "0.0418",
            "ts": "1718500000000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| ccy | String | 收益货币，固定为 `"USDT"` |
| earnAmt | String | 本次发放的 USDT 收益数量 |
| amt | String | 发放时点用户 USDT 本金余额 |
| apr | String | 本次发放适用的 APR，如 `"0.0418"` 表示 4.18% |
| ts | String | 发放时间戳，Unix 时间戳，单位为毫秒，如 `1597026383085` |
