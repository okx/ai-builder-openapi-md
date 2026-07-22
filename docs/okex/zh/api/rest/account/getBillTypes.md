---
site: okex
lang: zh
name: 获取账单类型
method: GET
path: /api/v5/account/subtypes
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取账单类型 [trading-account-rest-api-get-bill-types]

获取所有账单类型，以及账单类型（type）与子类型（subType）的映射关系。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/subtypes`

> 请求示例

```shell
GET /api/v5/account/subtypes
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:------|:-----|:--------|:----|
| type | String | 否 | 账单类型，支持多个，用英文逗号分隔，如 `1,2,3`；不填则返回所有类型。|

> 返回示例

```json
{
    "code": "0",
    "data": [
        {
            "type": "1",
            "typeDesc": "Transfer",
            "subTypeDetails": [
                {
                    "subType": "11",
                    "subTypeDesc": "Transfer in"
                },
                {
                    "subType": "12",
                    "subTypeDesc": "Transfer out"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
|:------|:-----|:----|
| type | String | 账单类型 |
| typeDesc | String | 账单类型描述，为 "" 代表该类型还未启用 |
| subTypeDetails | Array of objects | 子类型详情列表 |
| > subType | String | 子类型 |
| > subTypeDesc | String | 子类型描述，为 "" 代表该类型还未启用 |
