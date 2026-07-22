---
site: my
lang: zh
name: GET / 获取活期简单赚币余额
method: GET
path: /api/v5/finance/savings/balance
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取活期简单赚币余额 [financial-product-savings-get-saving-balance]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/savings/balance`

> 请求示例

```shell
GET /api/v5/finance/savings/balance?ccy=BTC

```

```python
import okx.Finance.Savings as Savings

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

SavingsAPI = Savings.SavingsAPI(apikey, secretkey, passphrase, False, flag)

result = SavingsAPI.get_saving_balance(ccy="USDT")
print(result)
```

#### 请求参数
| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| ccy  | String | 否       | 币种，如 `BTC` |

> 返回结果

```json
{
    "code": "0",
	"msg":"",
    "data": [
        {
            "earnings": "0.0010737388791526",
            "redemptAmt": "",
            "rate": "0.0100000000000000",
            "ccy": "USDT",
            "amt": "11.0010737453457821",
            "loanAmt": "11.0010630707982819",
            "pendingAmt": "0.0000106745475002"
        }
    ]
}
```

#### 返回参数

| 参数名    | 类型   | 描述                     |
| :-------- | :----- |:-----------------------|
| ccy       | String | 币种，如 `BTC`             |
| amt       | String | 币种数量                   |
| earnings  | String | 币种持仓收益                 |
| rate  | String | 用户配置的最低年化出借利率   |
| loanAmt  | String | 已出借数量                  |
| pendingAmt  | String | 未出借数量                  |
| redemptAmt  | String | <del>赎回中的数量</del>（已废弃） |
