---
site: my
lang: zh
name: 获取资金账户余额
method: GET
path: /api/v5/asset/balances
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取资金账户余额 [funding-account-rest-api-get-balance]

获取资金账户所有资产列表，查询各币种的余额、冻结和可用等信息。

<aside class="notice">
只返回余额大于0的币资产信息。
</aside>  

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/asset/balances`

> 请求示例

```shell
GET /api/v5/asset/balances

```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 获取资金账户余额
result = fundingAPI.get_balances()
print(result)
```

#### 请求参数
| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
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
| frozenBal | String | 冻结余额       |
| availBal  | String | 可用余额       |
