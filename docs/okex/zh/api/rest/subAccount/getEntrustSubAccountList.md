---
site: okex
lang: zh
name: 查看被托管的子账户列表
method: GET
path: /api/v5/users/entrust-subaccount-list
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 查看被托管的子账户列表 [sub-account-rest-api-get-custody-trading-sub-account-list]

交易团队使用该接口查看当前托管中的子账户列表

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/users/entrust-subaccount-list`

> 请求示例

```shell
GET /api/v5/users/entrust-subaccount-list

```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看被托管的子账户列表
result = subAccountAPI.get_entrust_subaccount_list()
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | 否       | 子账户名称   |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
       {
          "subAcct":"test-1"
       },
       {
          "subAcct":"test-2"
       }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct    | String   | 子账户名称                                                  |
