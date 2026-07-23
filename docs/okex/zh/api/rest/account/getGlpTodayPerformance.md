---
site: okex
lang: zh
name: 获取 GLP 当日表现
method: GET
path: /api/v5/users/glp/today-performance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取 GLP 当日表现 [trading-account-rest-api-get-glp-today-performance]

获取当前账户在所有已加入 GLP 业务线（Spot / Perp / Expiry & Nitro）的当日和月度累计（MTD）表现快照。无需请求参数，账户由 API key 自动解析。仅已加入且在有效期的 GLP 做市商账户可调用；子账户解析到其 master account。

#### 限速：5次/2s
#### 限速规则：User ID
#### 权限：读取

#### HTTP请求

`GET /api/v5/users/glp/today-performance`

> 请求示例

```shell
GET /api/v5/users/glp/today-performance
```

#### 请求参数

无。账户由登录态自动解析。

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "dataReady": true,
            "dataDate": "2026-07-13",
            "account": {
                "masterAccountId": "832545488879789797",
                "combinedAccountIds": ["832545488879789798"]
            },
            "programs": [
                {
                    "program": "SPOT",
                    "marketMakerBusinessId": "1",
                    "enrollmentStatus": "ENROLLED",
                    "marketMakerLevelId": "42",
                    "enrolledTierDisplay": "Tier 1 Class A",
                    "qualifyingPool": "TYPE_A",
                    "qualifyingRows": ["TOTAL"],
                    "daily": {
                        "volume": {
                            "typeA": {"maker": "1000000.00", "taker": "1000000.00"},
                            "typeBTotal": {"maker": "1000000.00", "taker": "1000000.00"},
                            "tradfiX2": {"maker": "1000000.00", "taker": "1000000.00"},
                            "total": {"maker": "2000000.00", "taker": "2000000.00"}
                        },
                        "share": {
                            "typeA": {"maker": "0.0000", "taker": "0.0000"},
                            "typeBAdj": {"maker": "0.0000", "taker": "0.0000"},
                            "total": {"maker": "0.0000", "taker": "0.0000"}
                        }
                    },
                    "mtd": {
                        "volume": {
                            "typeA": {"maker": "30000000.00", "taker": "30000000.00"},
                            "typeBTotal": {"maker": "30000000.00", "taker": "30000000.00"},
                            "tradfiX2": {"maker": "30000000.00", "taker": "30000000.00"},
                            "total": {"maker": "60000000.00", "taker": "60000000.00"}
                        },
                        "share": {
                            "typeA": {"maker": "0.0000", "taker": "0.0000"},
                            "typeBAdj": {"maker": "0.0000", "taker": "0.0000"},
                            "total": {"maker": "0.0000", "taker": "0.0000"}
                        },
                        "mtdStatus": "QUALIFIED",
                        "qualifyingShare": {"maker": "0.0000", "taker": "0.0000"}
                    }
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| dataReady   | Boolean  | 该 `dataDate` 是否已有数据。为 `false` 时 `programs` 为空数组 |
| dataDate    | String   | 数据快照日期，`yyyy-MM-dd` 格式（UTC+8）。通常为 T-1；T-1 计算未完成时回退 T-2 |
| account     | Object   | 账户身份信息 |
| > masterAccountId | String | master account ID |
| > combinedAccountIds | Array of strings | 同机构组的兄弟账户 ID（不含自己）。无组则为空数组 |
| programs    | Array of objects | 各已加入 GLP 业务线的表现数据。`dataReady` 为 `false` 时为空数组 |
| > program   | String   | GLP 业务线标识。<br>`SPOT`：现货<br>`PERP`：永续合约<br>`FUT_NTO`：交割合约 & Nitro |
| > marketMakerBusinessId | String | 该业务线的做市商 business ID |
| > enrollmentStatus | String | 加入状态。当前恒为 `ENROLLED` |
| > marketMakerLevelId | String | 当前档位 ID |
| > enrolledTierDisplay | String | 当前档位展示名，如 `Tier 1 Class A` |
| > qualifyingPool | String | 决定当前档位的池。<br>`TYPE_A`<br>`TYPE_B_ADJ`<br>`TYPE_A_AND_B` |
| > qualifyingRows | Array of strings | 合格行 key，如 `["TOTAL"]` |
| > daily     | Object   | 当日表现快照。包含 `volume` 和 `share`（结构见下方说明） |
| > mtd       | Object   | 月度累计表现。包含 `volume`、`share`（同 `daily` 结构），以及以下额外字段 |
| >> mtdStatus | String  | MTD 档位状态。<br>`QUALIFIED`：达标<br>`UPGRADE`：升档<br>`DOWNGRADE`：降档 |
| >> qualifyingShare | Object | 决定档位的池的份额。包含 `maker`（String）和 `taker`（String） |

**交易量和份额结构**

`daily` 和 `mtd` 均包含 `volume`（交易量）和 `share`（份额）两个 Object。每个 Object 下含分类 key，每个分类为包含 `maker`（String）和 `taker`（String）字段的 Object。

| **分类** | **在 `volume` 中** | **在 `share` 中** | **描述** |
| :------- | :----------------- | :---------------- | :------- |
| typeA      | 是 | 是 | Type A。`FUT_NTO` 时为 `null` |
| typeBTotal | 是 | 否 | Type B 合计。`FUT_NTO` 时为 `null` |
| typeBAdj   | 否 | 是 | Type B 调整后。`FUT_NTO` 时为 `null` |
| tradfiX2   | 是 | 否 | TradFi 量（已 ×2）。`FUT_NTO` 时为 `null` |
| total      | 是 | 是 | 各类型合计。始终存在 |

- **`volume`** 值：美元名义量，String，保留 2 位小数（如 `"1000000.00"`）
- **`share`** 值：小数字符串，4 位小数，无 `%` 后缀（如 `"0.0012"`）
