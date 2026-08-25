---
site: okex
lang: zh
name: GET / 获取余额
method: GET
path: /api/v5/finance/staking-defi/eth/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取余额 [financial-product-eth-staking-get-balance]

该余额表示账户内 BETH 的实时总持仓，包括交易账户、资金账户以及处于赎回过程中的资产。

#### 限速：6 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/eth/balance`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/eth/balance

```

```python
import okx.Finance.EthStaking as EthStaking

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_balance()
print(result)
```

#### 请求参数

None

> 返回结果

```json
{
    "code": "0",
    "data": [
      {
        "amt": "0.63926191",
        "ccy": "BETH",
        "latestInterestAccrual": "0.00006549",
        "totalInterestAccrual": "0.01490596",
        "ts": "1699257600000"
      }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                   | 类型     | 描述                                         |
|-----------------------|--------|--------------------------------------------|
| ccy                   | String | 币种名称，如 `BETH`                              |
| amt                   | String | 币种数量                                       |
| latestInterestAccrual | String | 最近收益              |
| totalInterestAccrual  | String | 历史总收益                                      |
| ts                    | String | 快照时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
