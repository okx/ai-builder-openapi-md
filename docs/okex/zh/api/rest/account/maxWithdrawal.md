---
site: okex
lang: zh
name: 查看账户最大可转余额
method: GET
path: /api/v5/account/max-withdrawal
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 查看账户最大可转余额 [trading-account-rest-api-get-maximum-withdrawals]

当指定币种时会返回该币种的交易账户到资金账户的最大可划转数量，不指定币种会返回所有拥有的币种资产可划转数量。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/max-withdrawal`

> 请求示例

```shell
GET /api/v5/account/max-withdrawal

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户最大可转余额
result = accountAPI.get_max_withdrawal()
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述           |
| :----- | :----- | :------- | :------------- |
| ccy    | String | 否       | 币种，如 `BTC`<br>支持多币种查询（不超过20个），币种之间半角逗号分隔 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"ccy": "BTC",
			"maxWd": "124",
			"maxWdEx": "125",
			"spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
		},
		{
			"ccy": "ETH",
			"maxWd": "10",
			"maxWdEx": "12",
			"spotOffsetMaxWd": "",
            "spotOffsetMaxWdEx": ""
		}
	]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                 |
| :--------- | :------- |:---------------------------------------|
| ccy        | String   | 币种                                     |
| maxWd      | String   | 最大可划转数量（不包含 `跨币种保证金模式`/`组合保证金模式` 借币金额） |
| maxWdEx      | String   | 最大可划转数量（包含 `跨币种保证金模式`/`组合保证金模式` 借币金额）  |
| spotOffsetMaxWd      | String   | 现货对冲不支持借币最大可转数量<br>仅适用于`组合保证金模式`       |
| spotOffsetMaxWdEx     | String   | 现货对冲支持借币的最大可转数量<br>仅适用于`组合保证金模式`       |
