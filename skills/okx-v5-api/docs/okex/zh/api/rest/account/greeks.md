---
site: okex
lang: zh
name: 查看账户Greeks
method: GET
path: /api/v5/account/greeks
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看账户Greeks [trading-account-rest-api-get-greeks]

获取账户资产的greeks信息。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/greeks`

> 请求示例

```shell
# 获取账户中所有资产的greeks
GET /api/v5/account/greeks

# 获取账户中BTC的greeks
GET /api/v5/account/greeks?ccy=BTC

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户Greeks
result = accountAPI.get_greeks()
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| ccy    | String | 否       | 币种，如 `BTC` |

> 返回结果

```json
{
    "code":"0",
    "data":[
        {            
           "thetaBS": "",
           "thetaPA":"",
           "deltaBS":"",
           "deltaPA":"",
           "gammaBS":"",
           "gammaPA":"",
           "vegaBS":"",    
           "vegaPA":"",
           "ccy":"BTC",
           "ts":"1620282889345"
        }
    ],
    "msg":""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                     |
| ----------- | -------- | ------------------------------------------------------------ |
| deltaBS       | String   | 美金本位账户资产delta     |
| deltaPA       | String   | 币本位账户资产delta                                                 |
| gammaBS       | String   | 美金本位账户资产gamma，仅适用于`期权` |
| gammaPA       | String   | 币本位账户资产gamma，仅适用于`期权`        |
| thetaBS       | String   | 美金本位账户资产theta，仅适用于`期权`          |
| thetaPA       | String   | 币本位账户资产theta，仅适用于`期权`          |
| vegaBS        | String   | 美金本位账户资产vega，仅适用于`期权`           |
| vegaPA        | String   | 币本位账户资产vega，仅适用于`期权`            |
| ccy           | String   | 币种  |
| ts            | String   | 获取greeks的时间，Unix时间戳的毫秒数格式，如 1597026383085     |
