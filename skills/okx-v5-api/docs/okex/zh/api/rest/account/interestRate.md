---
site: okex
lang: zh
name: 获取用户当前市场借币利率
method: GET
path: /api/v5/account/interest-rate
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取用户当前市场借币利率  [trading-account-rest-api-get-interest-rate]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/interest-rate`

> 请求示例

```shell
GET /api/v5/account/interest-rate

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取用户当前市场借币利率
result = accountAPI.get_interest_rate()
print(result)
```

#### 请求参数

| **参数名** | **类型** | 是否必须 | 描述                                                      |
| :--------- | :------- | :------- | :-------------------------------------------------------- |
| ccy        | String   | 否     | 币种                |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ccy":"BTC",
            "interestRate":"0.0001"
        },
        {
            "ccy":"LTC",
            "interestRate":"0.0003"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**       |
| :--------- | :------- |:-------------|
| interestRate     | String   | 每小时借币利率   |
| ccy              | String   | 币种           |
