---
site: okex
lang: zh
name: 创建子账户的API Key
method: POST
path: /api/v5/users/subaccount/apikey
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 创建子账户的API Key [sub-account-rest-api-create-an-api-key-for-a-sub-account]

仅适用于母账户，且母账户APIKey必须绑定IP。

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/users/subaccount/apikey`

> 请求示例

```shell
POST /api/v5/users/subaccount/apikey
body
{
    "subAcct":"panpanBroker2",
    "label":"broker3",
    "passphrase": "******",
    "perm":"trade"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                                                                                                |
| :------ | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subAcct 		| String | 是       | 子账户名称，支持6-20位字母和数字组合（区分大小写，不支持空格符号）                                                                                                                                               |
| label   		| String | 是       | API Key的备注                                                                                                                                                                        |
| passphrase  	| String | 是   	| API Key密码，8-32位字母数字组合，至少包含一个数字、一个大写字母、一个小写字母、一个特殊字符                                                                                                                               |
| perm    		| String | 否       | API Key权限<br>`read_only`：读取<br>`trade`：交易                                                                                                                                         |
| ip      		| String | 否       | 绑定ip地址，多个ip用半角逗号隔开，最多支持20个ip<br/><font color="red"><b>安全性考虑，推荐绑定IP</b></font> <br><font color="red"><b>未绑定IP且拥有交易或提币权限的API key，将在闲置14天之后自动删除。(模拟盘的API key不会被删除)</b></font>             |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "test-1",
		"label": "v5",
		"apiKey": "******",
		"secretKey": "******",
        "passphrase": "******",
		"perm": "read_only,trade",
		"ip": "1.1.1.1,2.2.2.2",
		"ts": "1597026383085"
	}]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct | String |  子账户名称   |
| label   | String | APIKey的备注|
| apiKey   | String | API公钥|
| secretKey   | String | API的私钥|
| passphrase  | String |APIKey的密码|
| perm    | String |  APIKey权限    |
| ip      | String | APIKey绑定的ip地址  |
| ts      | String | 创建时间 |
