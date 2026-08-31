---
site: okex
lang: zh
name: 获取交易所列表（公共）
method: GET
path: /api/v5/asset/exchange-list
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取交易所列表（公共）[funding-account-rest-api-get-exchange-list-public]

公共接口无须鉴权

#### 限速：6次/s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/asset/exchange-list`

> 请求示例

```shell
GET /api/v5/asset/exchange-list
```

```python

```

#### 请求参数

无

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "exchId": "did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1",
        "exchName": "1xbet"
    }
  ]
}
```

#### 返回参数

| 参数名    | 类型   | 描述            |
| :-------- | :----- |:--------------|
| exchName       | String | 交易所名称，如 `1xbet` |
| exchId       | String | 交易所 ID，如 `did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1`     |
