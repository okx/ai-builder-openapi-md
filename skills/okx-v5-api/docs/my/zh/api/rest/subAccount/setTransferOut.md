---
site: my
lang: zh
name: 设置子账户主动转出权限
method: POST
path: /api/v5/users/subaccount/set-transfer-out
module: users
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 设置子账户主动转出权限 [sub-account-rest-api-set-permission-of-transfer-out]

设置子账户转出权限（仅适用于母账户），默认可转出至母账户。

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/users/subaccount/set-transfer-out`

> 请求示例

```shell
POST /api/v5/users/subaccount/set-transfer-out
body
{
    "subAcct": "Test001,Test002",
    "canTransOut": true
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

# 设置子账户主动转出权限
result = subAccountAPI.set_permission_transfer_out(
    subAcct="hahawang1",
    canTransOut=False
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                  |
| :------ | :----- |:-----|:----------------------------------------------------|
| subAcct  | String | 是    | 子账户名称，支持设置多个（不超过20个），子账户名称之间半角逗号分隔                  |
| canTransOut | Boolean | 否    | 是否可以主动转出，默认为`true`<br>`false`：不可转出<br>`true`：可以转出            |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "subAcct": "Test001",
            "canTransOut": true
        },
        {
            "subAcct": "Test002",
            "canTransOut": true
        }
    ]
}

```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct     | String   | 子账户名称                 |
| canTransOut     | Boolean   | 是否可以主动转出 <br>`false`：不可转出<br>`true`：可以转出                  |
