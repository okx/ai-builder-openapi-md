---
site: okex
lang: zh
name: 开通期权交易
method: POST
path: /api/v5/account/activate-option
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 开通期权交易 [trading-account-rest-api-activate-option]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/activate-option`

> 请求示例

```shell
POST /api/v5/account/activate-option

```

#### 请求参数
无

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "ts": "1600000000000"
    }]
}
```

#### 返回参数

| 参数名  | 类型   | 描述       |
| :------ | :----- | :--------- |
| ts   | String | 开通时间   |
