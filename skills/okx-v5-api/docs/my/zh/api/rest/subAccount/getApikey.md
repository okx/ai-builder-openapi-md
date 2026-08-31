---
site: my
lang: zh
name: 查询子账户的API Key
method: GET
path: /api/v5/users/subaccount/apikey
module: users
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 查询子账户的API Key [sub-account-rest-api-query-the-api-key-of-a-sub-account]

仅适用于母账户  

#### 限速：20次/2秒
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/users/subaccount/apikey`

> 请求示例

```shell
GET /api/v5/users/subaccount/apikey?subAcct=panpanBroker2
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | 是       | 子账户名称   |
| apiKey  | String | 否       |API的公钥|

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "label":"v5",
            "apiKey":"arg13sdfgs",
            "perm":"read_only,trade",
            "ip":"1.1.1.1,2.2.2.2",
            "ts":"1597026383085"
        },
        {
            "label":"v5.1",
            "apiKey":"arg13sdfgs",
            "perm":"read_only",
            "ip":"1.1.1.1,2.2.2.2",
            "ts":"1597026383085"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                 |
| :--------- | :------- |:---------------------------------------|
| label   | String | API Key的备注                             |
| apiKey   | String | API Key公钥                              |
| perm    | String | API Key权限<br>read_only：读取<br>trade ：交易 |
| ip      | String | API Key绑定的ip地址                         |
| ts      | String | 创建时间                                   |
