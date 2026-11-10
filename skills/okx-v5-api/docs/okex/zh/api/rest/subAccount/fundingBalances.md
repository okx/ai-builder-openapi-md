---
site: okex
lang: zh
name: 获取子账户资金账户余额
method: GET
path: /api/v5/asset/subaccount/balances
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取子账户资金账户余额 [sub-account-rest-api-get-sub-account-funding-balance]

获取子账户资金账户余额（适用于母账户）

#### 限速：6次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/asset/subaccount/balances`

> 请求示例

```shell
GET /api/v5/asset/subaccount/balances?subAcct=test1

```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取子账户资金账户余额
result = subAccountAPI.get_funding_balance(
    subAcct="hahawang1"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | 是        | 子账户名称   |
| ccy  | String | 否       | 币种，如 `BTC`<br>支持多币种查询（不超过20个），币种之间半角逗号分隔 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"availBal": "37.11827078",
			"bal": "37.11827078",
			"ccy": "ETH",
			"frozenBal": "0"
		}
	]
}
```

#### 返回参数

| 参数名    | 类型   | 描述         |
| :-------- | :----- |:-----------|
| ccy       | String | 币种，如 `BTC` |
| bal       | String | 余额         |
| frozenBal | String | 冻结余额（不可用）  |
| availBal  | String | 可用余额       |
