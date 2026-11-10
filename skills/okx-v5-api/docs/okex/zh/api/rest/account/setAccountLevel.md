---
site: okex
lang: zh
name: 设置账户模式
method: POST
path: /api/v5/account/set-account-level
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置账户模式 [trading-account-rest-api-set-account-mode]

账户模式的首次设置，需要在网页或手机app上进行。若用户计划在持有仓位的情况下切换账户模式，应该先调用预设置接口进行必要的预设置，再调用预检查接口获取不匹配信息、保证金校验等相关信息，最后调用账户模式切换接口进行账户模式切换。

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-account-level`

> 请求示例

```shell
POST /api/v5/account/set-account-level
body
{
    "acctLv":"1"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                     |
| :------ | :----- | :------- |:-----------------------------------------------------------------------|
| acctLv  | String | 是     | 账户模式<br>`1`: 现货模式<br>`2`: 合约模式 <br>`3`: 跨币种保证金模式 <br>`4`: 组合保证金模式 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data" :[
          {
            "acctLv":"1"
          }
    ]  
}
```

#### 返回参数

| 参数名  | 类型   | 描述   |
| :------ | :----- |:-----|
| acctLv   | String | 账户模式 |
