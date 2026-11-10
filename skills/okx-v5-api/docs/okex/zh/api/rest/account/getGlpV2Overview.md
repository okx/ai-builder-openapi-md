---
site: okex
lang: zh
name: 获取 GLP v2 概览
method: GET
path: /api/v5/users/glp/v2/overview
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取 GLP v2 概览 [trading-account-rest-api-get-glp-v2-overview]

获取当前账户的 GLP 注册信息、当前等级、预估等级、等级要求及本周绩效。子账户 API key 获取的是主账户的 GLP 数据。

#### 限速：5次/2s
#### 限速规则：User ID
#### 权限：读取

#### HTTP请求

`GET /api/v5/users/glp/v2/overview`

> 请求示例

```shell
GET /api/v5/users/glp/v2/overview
```

#### 请求参数

无请求参数。

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "programs": [
                {
                    "bizId": "16",
                    "program": "GLP_B",
                    "side": "MAKER",
                    "instrument": "PERP",
                    "currentTier": "Tier 2",
                    "estNextTier": "Tier 2",
                    "estNextStatus": "qualify",
                    "gracePeriodEndTime": "",
                    "requirement": [
                        {
                            "tier": "Tier 1",
                            "share": "0.005"
                        },
                        {
                            "tier": "Tier 2",
                            "share": "0.003"
                        }
                    ],
                    "performance": [
                        {
                            "date": "1726531200000",
                            "assessPeriod": "PREVIOUS",
                            "share": "0.0035",
                            "vol": "140000"
                        },
                        {
                            "date": "1726876800000",
                            "assessPeriod": "CURRENT",
                            "share": "0.004",
                            "vol": "200"
                        }
                    ]
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| programs | Array of objects | 当前 GLP 注册信息。无可用数据时返回空数组。 |
| > bizId | String | 产品及交易方向标识。<br>`14`：SPOT MAKER<br>`15`：FUT_NTO MAKER<br>`16`：PERP MAKER<br>`17`：SPOT TAKER<br>`18`：FUT_NTO TAKER<br>`19`：PERP TAKER |
| > program | String | GLP 项目名称。<br>`GLP_A`<br>`GLP_B` |
| > side | String | 交易方向。<br>`MAKER`<br>`TAKER` |
| > instrument | String | GLP 产品类别。<br>`SPOT`：现货<br>`PERP`：永续合约<br>`FUT_NTO`：交割合约及 Nitro |
| > currentTier | String | 当前生效的 GLP 等级。不可用时返回空字符串。 |
| > estNextTier | String | 基于最新考核的预估 GLP 等级。不可用时返回空字符串。 |
| > estNextStatus | String | 预估等级结果。<br>`disqualify`<br>`downgrade`<br>`qualify`<br>`upgrade`<br>不可用时返回空字符串。 |
| > gracePeriodEndTime | String | 降级最早生效日期。Unix 毫秒时间戳。不适用时返回空字符串。 |
| > requirement | Array of objects | GLP 等级的市场份额门槛。 |
| >> tier | String | GLP 等级名称。 |
| >> share | String | 所需市场份额，以小数表示。`"0.007"` = 0.7%。不可用时为 `null`。 |
| > performance | Array of objects | 按交易日统计的本周绩效。 |
| >> date | String | 交易日。Unix 毫秒时间戳。 |
| >> assessPeriod | String | 该交易日所属的考核周期。<br>`PREVIOUS`<br>`CURRENT` |
| >> share | String | 本周市场份额，以小数表示。不可用时为 `null`。 |
| >> vol | String | 本周交易量。不可用时为 `null`。 |
