---
site: okex
lang: zh
name: 获取 GLP v2 历史绩效
method: GET
path: /api/v5/users/glp/v2/history/performance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取 GLP v2 历史绩效 [trading-account-rest-api-get-glp-v2-performance-history]

获取所选 GLP 产品及交易方向的每周考核结果、等级变更和绩效指标。子账户 API key 获取的是主账户的 GLP 数据。

#### 限速：5次/2s
#### 限速规则：User ID
#### 权限：读取

#### HTTP请求

`GET /api/v5/users/glp/v2/history/performance`

> 请求示例

```shell
GET /api/v5/users/glp/v2/history/performance?bizId=16
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| :--------- | :------- | :----------- | :------- |
| bizId | String | 是 | 产品及交易方向标识。<br>`14`：SPOT MAKER<br>`15`：FUT_NTO MAKER<br>`16`：PERP MAKER<br>`17`：SPOT TAKER<br>`18`：FUT_NTO TAKER<br>`19`：PERP TAKER |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "performance": [
                {
                    "assessStartTime": "1726185600000",
                    "assessEndTime": "1726704000000",
                    "tier": "Tier 3",
                    "nextTier": "Tier 2",
                    "accOrigVol": "100000",
                    "accOrigShare": "0.004",
                    "accAdjVol": "200000",
                    "accAdjShare": "0.004",
                    "assessResult": "upgrade"
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| performance | Array of objects | 每周考核结果。无历史数据时返回空数组。 |
| > assessStartTime | String | 考核周期开始时间。Unix 毫秒时间戳。 |
| > assessEndTime | String | 考核周期结束时间。Unix 毫秒时间戳。 |
| > tier | String | 本次考核使用的 GLP 等级。 |
| > nextTier | String | 本次考核确定的等级。不可用时返回空字符串。 |
| > accOrigVol | String | 考核周期内加权前的交易量。不可用时为 `null`。 |
| > accOrigShare | String | 考核周期内市场份额，以小数表示。不可用时为 `null`。 |
| > accAdjVol | String | 考核周期内调整后的交易量。不可用时为 `null`。 |
| > accAdjShare | String | 考核周期内调整后的市场份额，以小数表示。不可用时为 `null`。 |
| > assessResult | String | 等级变更结果。<br>`disqualify`<br>`downgrade`<br>`qualify`<br>`upgrade`<br>不可用时返回空字符串。 |
