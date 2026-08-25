---
site: okex
lang: zh
name: POST / 设置活期简单赚币借贷利率
method: POST
path: /api/v5/finance/savings/set-lending-rate
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 设置活期简单赚币借贷利率 [financial-product-savings-post-set-lending-rate]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/savings/set-lending-rate`

> 请求示例

```shell
POST /api/v5/finance/savings/set-lending-rate
body
{
    "ccy":"BTC",
    "rate":"0.02"
}

```

```python
import okx.Finance.Savings as Savings

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.set_lending_rate(ccy='USDT',rate="1")
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                        |
| ------ | ------ | -------- |---------------------------|
| ccy    | String | 是       | 币种名称，如 `BTC`              |
| rate   | String | 是       | 贷出年利率<br>参数取值范围在1%到365%之间 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"rate": "0.02"
	}]
}
```

#### 返回参数

| 参数名 | 类型   | 描述               |
| ------ | ------ | ------------------ |
| ccy    | String | 币种名称，如 `BTC`                           |
| rate   | String | 贷出年利率 |
