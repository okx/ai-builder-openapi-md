---
site: okex
lang: zh
name: 获取组合保证金模式仓位限制
method: GET
path: /api/v5/account/position-tiers
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取组合保证金模式仓位限制 [trading-account-rest-api-get-pm-position-limitation]
仅支持获取组合保证金模式下，交割、永续和期权的全仓仓位限制。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/position-tiers`

> 请求示例

```shell
# 查看BTC-USDT在组合保证金模式下的全仓限制
GET /api/v5/account/position-tiers?instType=SWAP&uly=BTC-USDT

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取组合保证金模式仓位限制
result = accountAPI.get_account_position_tiers(
    instType="SWAP",
    uly="BTC-USDT"
)
print(result)
```

#### 请求参数 

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                    |
| :------- | :----- |:-----|:----------------------------------------------------------------------------------------------------------------------|
| instType | String | 是    | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>                                                       |
| instFamily   | String | 是    | 交易品种，如 `BTC-USDT`，支持多个查询（不超过5个），`instFamily`之间半角逗号分隔|

> 返回结果

```json
{
  "code": "0",
  "data": [
    {
      "instFamily": "BTC-USD",
      "maxSz": "10000",
      "posType": "",
      "uly": "BTC-USDT"
    }
  ],
  "msg": ""
}
```

#### 返回参数 

| 参数名   | 类型   | 描述                                                                                                                                        |
| :------- | :----- |:------------------------------------------------------------------------------------------------------------------------------------------|
| uly | String | 标的指数<br>适用于`交割`/`永续`/`期权`                                                                                                                                      |
| instFamily | String | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                                                                      |
| maxSz  | String | 最大持仓量                                                                                                                                     |
| posType      | String | 限仓类型，仅适用于组合保证金模式下的期权全仓。<br>`1`：所有合约挂单 + 持仓张数，`2`：所有合约总挂单张数，`3`：所有合约总挂单单数，`4`：同方向合约挂单 + 持仓张数，`5`：单一合约总挂单单数，`6`：单一合约挂单 + 持仓张数，`7`：单笔挂单张数" |
