---
site: turkey
lang: zh
name: 创建子账户
method: POST
path: /api/v5/users/subaccount/create-subaccount
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 创建子账户 [sub-account-rest-api-create-sub-account]

仅适用于母账户，且母账户APIKey必须绑定IP。

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/users/subaccount/create-subaccount`

> 请求示例

```shell
POST /api/v5/users/subaccount/create-subaccount
body
{
    "subAcct": "subAccount002",
    "type": "1",
    "label": "123456"
}
```

#### 请求参数

| 参数名        | 类型   | 是否必须   | 描述                                                                                                                              |
|:-----------| :----- |:-------|:--------------------------------------------------------------------------------------------------------------------------------|
| subAcct 		 | String | 是      | 子账户名称，支持6-20位字母和数字组合（区分大小写，不支持空格符号）                                                                                             |
| type 		    | String | 是      | 子账户类型 <br>`1`：普通子账户 <br>`5`：托管交易子账户 - Copper<br> `12`：托管交易子账户 - Komainu                                                         |
| label   		| String | 是      | API Key的备注，支持6-32位字母（区分大小写），数字，或者特殊字符如: *                                                                                       |
| pwd  	| String | 可选   	 | 子账户登录密码，仅 KYB 账户必填<br/> 您的密码必须满足以下条件：<br/>长度为 8 ~ 32 个字符。<br/>1 个小写字母 (a-z) <br/>1 个大写字母 (A-Z) <br/>1 个数字 <br/>1 个符号，如：！@ # $ % |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "123456",
            "subAcct": "subAccount002",
            "ts": "1744875304520",
            "uid": "698827017768230914"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**        |
| :--------- | :------- |:--------------|
| subAcct | String | 子账户名称         |
| label   | String | 子账户的备注        |
| uid   | String | 子账户 ID        |
| ts      | String | 创建时间          |
