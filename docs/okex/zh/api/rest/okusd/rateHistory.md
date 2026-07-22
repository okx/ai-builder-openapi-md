---
site: okex
lang: zh
name: GET / 查询 APR 历史
method: GET
path: /api/v5/finance/okusd/rate/history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查询 APR 历史 [financial-product-okusd-get-apr-history]

查询 OKUSD 历史 APR 快照。结果按时间戳倒序返回（最新优先）。虽然数据为产品级（非用户维度），本接口仍需 API Key 鉴权。

#### 限速：5次/2s
#### 限速规则：User ID
#### Permission: Read

#### HTTP 请求

`GET /api/v5/finance/okusd/rate/history`

> 请求示例

```shell
GET /api/v5/finance/okusd/rate/history?limit=10
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
        { "apr": "0.0418", "ts": "1718500000000" },
        { "apr": "0.0395", "ts": "1718413600000" }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| apr | String | 该快照时刻的 OKUSD APR，如 `"0.0418"` 表示 4.18% |
| ts | String | 快照时间戳，Unix 时间戳，单位为毫秒，如 `1597026383085` |
