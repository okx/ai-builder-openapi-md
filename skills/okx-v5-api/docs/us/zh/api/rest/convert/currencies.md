---
site: us
lang: zh
name: 获取闪兑币种列表
method: GET
path: /api/v5/asset/convert/currencies
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取闪兑币种列表 [funding-account-rest-api-get-convert-currencies]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/convert/currencies`

> 请求示例

```shell
GET /api/v5/asset/convert/currencies

```

#### 请求参数

无

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "min": "",  // 已废弃
            "max": "",  // 已废弃
            "ccy": "BTC"
        },
        {
            "min": "",
            "max": "",
            "ccy": "ETH"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名      | 类型    | 描述                       |
| ----------- | ------- |--------------------------|
| ccy         | String  | 币种名称，如 `BTC`             |
| min         | String  | <del>支持闪兑的最小值</del>(已废弃) |
| max         | String  | <del>支持闪兑的最大值</del>(已废弃) |
