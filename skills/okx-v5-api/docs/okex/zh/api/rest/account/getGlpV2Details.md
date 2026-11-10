---
site: okex
lang: zh
name: 获取 GLP v2 考核详情
method: GET
path: /api/v5/users/glp/v2/details
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取 GLP v2 考核详情 [trading-account-rest-api-get-glp-v2-assessment-details]

获取所选 GLP 产品及交易方向在当前考核周期内的每日和本周绩效。子账户 API key 获取的是主账户的 GLP 数据。

#### 限速：5次/2s
#### 限速规则：User ID
#### 权限：读取

#### HTTP请求

`GET /api/v5/users/glp/v2/details`

> 请求示例

```shell
GET /api/v5/users/glp/v2/details?bizId=16
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
            "assessStartTime": "1726790400000",
            "assessEndTime": "1727308800000",
            "performance": [
                {
                    "date": "1726790400000",
                    "origVol": "100",
                    "origShare": "0.004",
                    "adjVol": "200",
                    "adjShare": "0.004",
                    "accOrigVol": "100",
                    "accOrigShare": "0.004",
                    "accAdjVol": "200",
                    "accAdjShare": "0.004"
                },
                {
                    "date": "1726876800000",
                    "origVol": "200",
                    "origShare": "0.004",
                    "adjVol": "400",
                    "adjShare": "0.004",
                    "accOrigVol": "300",
                    "accOrigShare": "0.004",
                    "accAdjVol": "600",
                    "accAdjShare": "0.004"
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| assessStartTime | String | 考核周期开始时间。Unix 毫秒时间戳。 |
| assessEndTime | String | 考核周期结束时间。Unix 毫秒时间戳。 |
| performance | Array of objects | 每日及本周绩效。无可用数据时返回空数组。 |
| > date | String | 交易日。Unix 毫秒时间戳。 |
| > origVol | String | 加权前的每日交易量。不可用时为 `null`。 |
| > origShare | String | 每日市场份额，以小数表示。不可用时为 `null`。 |
| > adjVol | String | 调整后的每日交易量。不可用时为 `null`。 |
| > adjShare | String | 调整后的每日市场份额，以小数表示。不可用时为 `null`。 |
| > accOrigVol | String | 加权前的本周累计交易量。不可用时为 `null`。 |
| > accOrigShare | String | 本周累计市场份额，以小数表示。不可用时为 `null`。 |
| > accAdjVol | String | 调整后的本周累计交易量。不可用时为 `null`。 |
| > accAdjShare | String | 调整后的本周累计市场份额，以小数表示。不可用时为 `null`。 |
