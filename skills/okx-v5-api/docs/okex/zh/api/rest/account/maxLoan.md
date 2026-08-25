---
site: okex
lang: zh
name: 获取交易产品最大可借
method: GET
path: /api/v5/account/max-loan
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取交易产品最大可借 [trading-account-rest-api-get-the-maximum-loan-of-instrument]

#### 限速：20 次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/account/max-loan`

> 请求示例

```shell
# 现货模式用户已经开通了借币情况下币对币种最大可借
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross

# 现货模式用户已经开通了借币情况下币种最大可借
GET  /api/v5/account/max-loan?ccy=USDT&mgnMode=cross

# 合约模式逐仓账户获取币币杠杆最大可借
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=isolated

# 合约模式全仓账户获取币币杠杆最大可借（指定保证金为BTC）
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross&mgnCcy=BTC

# 跨币种全仓账户获取币币杠杠最大可借
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 合约模式全仓账户获取币币杠杆最大可借（指定保证金为BTC）
result = accountAPI.get_max_loan(
    instId="BTC-USDT",
    mgnMode="cross",
    mgnCcy="BTC"
)
print(result)
```

#### 请求参数

| 参数名     | 类型   | 是否必须     | 描述                                            |
|:--------| :----- |:---------|:----------------------------------------------|
| mgnMode | String | 是        | 仓位类型<br>`isolated`：逐仓<br>`cross`：全仓           |
| instId  | String | 可选       | 产品 ID，如 `BTC-USDT`<br>支持多产品ID查询（不超过5个），半角逗号分隔 |
| ccy     | String | 可选       | 币种<br>仅适用于`现货模式`下手动借币币种最大可借                   |
| mgnCcy  | String | 可选       | 保证金币种，如 `BTC`<br>适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`  |
| tradeQuoteCcy | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USDT",
      "mgnMode": "isolated",
      "mgnCcy": "",
      "maxLoan": "0.1",
      "ccy": "BTC",
      "side": "sell"
    },
    {
      "instId": "BTC-USDT",
      "mgnMode": "isolated",
      "mgnCcy": "",
      "maxLoan": "0.2",
      "ccy": "USDT",
      "side": "buy"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                       |
| :--------- | :------- | :----------------------------------------------------------------------------- |
| instId     | String   | 产品 ID                                                                        |
| mgnMode    | String   | 仓位类型                                                                       |
| mgnCcy     | String   | 保证金币种 |
| maxLoan    | String   | 最大可借                                                                       |
| ccy        | String   | 币种                                                                           |
| side       | String   | 订单方向                                                                       |
