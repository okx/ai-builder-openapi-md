---
site: my
lang: zh
name: 手动借/还币
method: POST
path: /api/v5/account/spot-manual-borrow-repay
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 手动借/还币 [trading-account-rest-api-manual-borrow-repay]

仅适用于`现货模式`已开通借币的情况。

#### 限速：1次/3s
#### 限速规则：Master Account User ID

#### HTTP请求

`POST /api/v5/account/spot-manual-borrow-repay`

> 请求示例

```shell
POST /api/v5/account/spot-manual-borrow-repay 
body
{
    "ccy": "USDT",
    "side": "borrow",
    "amt": "100"
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

result = accountAPI.spot_manual_borrow_repay(ccy="USDT", side="borrow", amt= "1")
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                              |
|:-----| :----- |:-----|:--------------------------------|
| ccy  | String | 是    | 币种，如 `BTC`                      |
| side | String | 是    | 方向<br>`borrow`：借币<br>`repay`：还币 |
| amt  | String | 是    | 数量                         |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy":"USDT",
            "side":"borrow",
            "amt":"100"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名  | 类型   | 描述                              |
| :------ | :----- |:--------------------------------|
| ccy  | String | 币种，如 `BTC`                      |
| side | String | 方向<br>`borrow`：借币<br>`repay`：还币 |
| amt  | String | 实际数量                            |
