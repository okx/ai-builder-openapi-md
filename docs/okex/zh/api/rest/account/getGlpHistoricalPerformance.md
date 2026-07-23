---
site: okex
lang: zh
name: 获取 GLP 历史表现
method: GET
path: /api/v5/users/glp/historical-performance
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取 GLP 历史表现 [trading-account-rest-api-get-glp-historical-performance]

获取单个 GLP 业务线的逐日表现记录，按日期降序排列（最新日期在前）。仅已加入且在有效期的 GLP 做市商账户可调用；子账户解析到其 master account。

#### 限速：5次/2s
#### 限速规则：User ID
#### 权限：读取

#### HTTP请求

`GET /api/v5/users/glp/historical-performance`

> 请求示例

```shell
GET /api/v5/users/glp/historical-performance?program=SPOT
GET /api/v5/users/glp/historical-performance?program=SPOT&begin=1751299200000&end=1753804800000&limit=31
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述 |
| :------ | :----- | :------- | :--- |
| program | String | 是       | GLP 业务线标识。<br>`SPOT`<br>`PERP`<br>`FUT_NTO` |
| begin   | String | 否       | 开始日期过滤（含）。Unix 毫秒字符串，如 `"1751299200000"`。默认：当月 1 号（UTC+8） |
| end     | String | 否       | 结束日期过滤（含）。Unix 毫秒字符串。默认：今天（UTC+8） |
| limit   | String | 否       | 每页最大记录数。默认 `"31"`，最大 `"100"` |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "date": "2026-07-13",
            "volume": {
                "typeA": {"maker": "1000000.00", "taker": "1000000.00"},
                "typeBTotal": {"maker": "1000000.00", "taker": "1000000.00"},
                "tradfiX2": {"maker": "1000000.00", "taker": "1000000.00"},
                "total": {"maker": "2000000.00", "taker": "2000000.00"}
            },
            "share": {
                "typeA": {"maker": "0.0012", "taker": "0.0010"},
                "typeBAdj": {"maker": "0.0008", "taker": "0.0007"},
                "total": {"maker": "0.0010", "taker": "0.0009"}
            }
        },
        {
            "date": "2026-07-12",
            "volume": {
                "typeA": {"maker": "950000.00", "taker": "980000.00"},
                "typeBTotal": {"maker": "850000.00", "taker": "900000.00"},
                "tradfiX2": {"maker": "800000.00", "taker": "820000.00"},
                "total": {"maker": "1800000.00", "taker": "1880000.00"}
            },
            "share": {
                "typeA": {"maker": "0.0011", "taker": "0.0009"},
                "typeBAdj": {"maker": "0.0007", "taker": "0.0006"},
                "total": {"maker": "0.0009", "taker": "0.0008"}
            }
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :--------- | :------- | :------- |
| date       | String   | 日期，`yyyy-MM-dd` 格式（UTC+8） |
| volume     | Object   | 各池类型的交易量（美元名义，2 位小数）。结构同当日表现接口的 `daily.volume` |
| share      | Object   | 各池类型的市场份额（小数字符串，4 位小数，无 `%` 后缀）。结构同当日表现接口的 `daily.share` |

**错误码**

| 错误码 | HTTP 状态码 | 错误提示 |
| :----- | :---------- | :------- |
| 50030  | 200         | 您无权使用此 API 端点 |
| 50014  | 200         | 参数 {param0} 不能为空 |
| 51000  | 200         | 参数错误 |
| 50016  | 200         | 参数 {param0} 与参数 {param1} 不匹配 |
