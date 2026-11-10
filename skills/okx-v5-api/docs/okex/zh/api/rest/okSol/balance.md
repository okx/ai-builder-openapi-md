---
site: okex
lang: zh
name: GET / 获取余额
method: GET
path: /api/v5/finance/staking-defi/sol/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取余额 [financial-product-sol-staking-get-balance]

该余额表示账户内 OKSOL 的实时总持仓，包括交易账户、资金账户以及处于赎回过程中的资产。

#### 限速：6 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/sol/balance`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/sol/balance

```

```python
import okx.Finance.SolStaking as SolStaking

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = SolStaking.SolStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.sol_balance()
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
            "amt": "0.01100012",
            "ccy": "OKSOL",
            "latestInterestAccrual": "0.00000012",
            "totalInterestAccrual": "0.00000012"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                   | 类型     | 描述             |
|-----------------------|--------|----------------|
| ccy                   | String | 币种名称，如 `OKSOL` |
| amt                   | String | 币种数量           |
| latestInterestAccrual | String | 最近收益           |
| totalInterestAccrual  | String | 历史总收益          |
