---
site: turkey
lang: zh
name: 删除子账户的API Key
method: POST
path: /api/v5/users/subaccount/delete-apikey
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 删除子账户的API Key [sub-account-rest-api-delete-the-api-key-of-sub-accounts]

仅适用于母账户，且母账户APIKey必须绑定IP。

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/users/subaccount/delete-apikey`

> 请求示例

```shell
POST /api/v5/users/subaccount/delete-apikey
body
{
    "subAcct":"test00001",
    "apiKey":"******"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | 是       | 子账户名称   |
| apiKey   | String | 是       | API的公钥|

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "test00001"
	}]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct | String |  子账户名称   |
