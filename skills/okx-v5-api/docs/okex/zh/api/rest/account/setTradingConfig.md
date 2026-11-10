---
site: okex
lang: zh
name: 设置交易配置
method: POST
path: /api/v5/account/set-trading-config
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置交易配置 [trading-account-rest-api-set-trading-config]

#### **限速：1次/2s**

#### **限速规则：User ID**

#### **HTTP请求**

`POST /api/v5/account/set-trading-config`

> 请求示例

```shell
POST /api/v5/account/set-trading-config
body
{
    "type": "stgyType",
    "stgyType":"1"
}

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| type | String | Yes | 交易配置类型<br> `stgyType` |
| stgyType | String | No | 账号策略类型<br> `0`：普通策略模式<br> `1`：delta 中性策略模式<br> 仅适用于type为`stgyType` |

> 返回示例

```json
{
   "code":"0",
   "msg":"",
   "data":[
      {
            "type": "stgyType",
            "stgyType":"1"
      }
   ]
}

```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| type | String | 交易配置类型 |
| stgyType | String | 账号策略类型 |
