---
site: okex
lang: zh
name: 期权greeks的PA/BS切换
method: POST
path: /api/v5/account/set-greeks
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 期权greeks的PA/BS切换 [trading-account-rest-api-set-greeks-pa-bs]

设置greeks的展示方式。

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-greeks`

> 请求示例

```shell
POST /api/v5/account/set-greeks 
body
{
    "greeksType":"PA"
}

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 期权greeks的PA/BS切换
result = accountAPI.set_greeks(greeksType="PA")
print(result)
```

#### 请求参数

| 参数名     | 类型   | 是否必须 | 描述                                           |
| :--------- | :----- | :------- | :--------------------------------------------- |
| greeksType | String | 是       | 希腊字母展示方式<br>`PA`：币本位，`BS`：美元本位 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"greeksType": "PA"
	}]
}
```

#### 返回参数

| 参数名     | 类型   | 描述                 |
| :--------- | :----- | :------------------- |
| greeksType | String | 当前希腊字母展示方式 |
