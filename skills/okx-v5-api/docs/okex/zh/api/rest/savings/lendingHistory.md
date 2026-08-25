---
site: okex
lang: zh
name: GET / 获取活期简单赚币出借明细
method: GET
path: /api/v5/finance/savings/lending-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取活期简单赚币出借明细  [financial-product-savings-get-lending-history]

返回最近一个月的数据

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/savings/lending-history`

> 请求示例

```shell
GET /api/v5/finance/savings/lending-history

```

```python
import okx.Finance.Savings as Savings

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.get_lending_history()
print(result)
```

#### 请求参数
| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| ccy  | String | 否       | 币种，如 `BTC` |
| after  | String | 否       | 查询在此之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| before | String | 否       | 查询在此之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| limit  | String | 否       | 分页返回的结果集数量，最大为 100，不填默认返回 100 条                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"ccy": "BTC",
			"amt": "0.01",
			"earnings": "0.001",
			"rate": "0.01",
			"ts": "1597026383085"
		},
		{
			"ccy": "ETH",
			"amt": "0.2",
			"earnings": "0.001",
			"rate": "0.01",
			"ts": "1597026383085"
		}
	]
}
```

#### 返回参数

| 参数名    | 类型   | 描述           |
| :-------- | :----- | :------------- |
| ccy       | String | 币种，如 `BTC` |
| amt       | String | 出借数量 |
| earnings  | String | 已赚取利息 |
| rate  | String | 出借年利率 |
| ts  | String | 出借时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
