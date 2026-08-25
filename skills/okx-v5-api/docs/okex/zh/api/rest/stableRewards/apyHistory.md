---
site: okex
lang: zh
name: GET / 获取历史收益率
method: GET
path: /api/v5/finance/stable-rewards/apy-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取历史收益率 [financial-product-stable-rewards-get-apy-history]

查询指定稳定币的历史每日年化收益率。返回值为用户当前 VIP 等级对应的收益率。

#### 限速：6次/s
#### 限速规则：IP

#### HTTP 请求

`GET /api/v5/finance/stable-rewards/apy-history`

> 请求示例

```shell
GET /api/v5/finance/stable-rewards/apy-history?ccy=USDG
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| ccy | String | 是 | 稳定币，如 `USDG` |
| days | String | 否 | 查询最近多少天的历史数据。默认 `100`，最大 `100` |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "rate": "0.004",
            "ts": "1718035200000"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| rate | String | 用户当前 VIP 等级对应的日度年化收益率，如 `0.041` 代表 `4.1%` |
| ts | String | 数据快照时间（UTC+0），Unix 时间戳，单位为毫秒，如 `1597026383085` |
