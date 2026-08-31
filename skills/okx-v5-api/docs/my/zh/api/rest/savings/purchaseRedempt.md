---
site: my
lang: zh
name: POST / 活期简单赚币申购/赎回
method: POST
path: /api/v5/finance/savings/purchase-redempt
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### POST / 活期简单赚币申购/赎回 [financial-product-savings-post-savings-purchase-redemption]

仅资金账户中的资产支持活期简单赚币申购。

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/savings/purchase-redempt`

> 请求示例

```shell
POST /api/v5/finance/savings/purchase-redempt
body
{
    "ccy":"BTC",
    "amt":"1",
    "side":"purchase",
    "rate":"0.01"
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

result = SavingsAPI.savings_purchase_redemption(ccy='USDT',amt="0.1",side="purchase",rate="1")
print(result)
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                  |
|--------| ------ |------|---------------------------------------------------------------------|
| ccy    | String | 是    | 币种名称，如 `BTC`                                                        |
| amt    | String | 是    | 申购/赎回 数量                                                            |
| side   | String | 是    | 操作类型<br>`purchase`：申购 `redempt`：赎回                                  |
| rate   | String | 可选   | 申购年利率，如 `0.1`代表`10%`<br>仅适用于申购，新申购的利率会覆盖上次申购的利率<br>参数取值范围在1%到365%之间 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ccy":"BTC",
            "amt":"1",
            "side":"purchase",
            "rate":"0.01"
        }
    ]
}
```

#### 返回参数

| 参数名 | 类型   | 描述                   |
| ------ | ------ |----------------------|
| ccy    | String | 币种名称                 |
| amt    | String | 申购/赎回 数量             |
| side   | String | 操作类型                 |
| rate   | String | 申购年利率，如 `0.1`代表`10%` |
