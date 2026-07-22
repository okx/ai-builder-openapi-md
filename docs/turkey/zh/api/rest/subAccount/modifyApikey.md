---
site: turkey
lang: zh
name: 重置子账户的APIKey
method: POST
path: /api/v5/users/subaccount/modify-apikey
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 重置子账户的APIKey [sub-account-rest-api-reset-the-api-key-of-a-sub-account]

仅适用于母账户，且母账户APIKey必须绑定IP。

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/users/subaccount/modify-apikey`

> 请求示例

```shell
POST /api/v5/users/subaccount/modify-apikey
body
{
    "subAcct":"yongxu",
    "apiKey":"******"
    "ip":"1.1.1.1"
}
```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 重置子账户的APIKey
result = subAccountAPI.reset_subaccount_apikey(
    subAcct="hahawang1",
    apiKey="",
    ip=""
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                |
| :------ | :----- | :------- |:----------------------------------------------------------------------------------|
| subAcct | String | 是       | 子账户名称                                                                             |
| apiKey  | String | 是       | 子账户API的公钥                                                                         |
| label   | String | 否       | 子账户APIKey的备注，如果填写该字段，则该字段会被重置                                                     |
| perm    | String | 否       | 子账户APIKey权限<br>`read_only`：读取<br>`trade`：交易<br>多个权限用半角逗号隔开。<br>如果填写该字段，则该字段会被重置。  |
| ip      | String | 否       | 子账户APIKey绑定ip地址，多个ip用半角逗号隔开，最多支持20个ip。<br>如果填写该字段，那该字段会被重置。<br>如果ip传""，则表示解除IP绑定。 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "yongxu",
		"label": "v5",
        "apiKey": "******",
		"perm": "read,trade",
		"ip": "1.1.1.1",
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
| perm    | String |  APIKey权限  |
| ip      | String | APIKey绑定的ip地址  |
| ts      | String | 创建时间 |
