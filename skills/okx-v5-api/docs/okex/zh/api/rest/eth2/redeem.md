---
site: okex
lang: zh
name: POST / 赎回
method: POST
path: /api/v5/finance/staking-defi/eth/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 赎回 [financial-product-eth-staking-post-redeem]

只能赎回资金账户中的 BETH 资产，交易账户中的 BETH 资产需要您先做资金划转到资金账户后赎回。

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/staking-defi/eth/redeem`

> 请求示例

```shell
POST /api/v5/finance/staking-defi/eth/redeem
body 
{
    "amt":"10"
}
```

```python
import okx.Finance.EthStaking as EthStaking

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_redeem(amt="1")
print(result)
```

#### 请求参数

| 参数    | 类型      | 是否必须 | 描述   |
|:------|:--------|:-----|:-----|
| amt   | String | 是    | 赎回数量 |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
  ]
}
```

#### 返回参数

code = `0`代表请求已被成功处理
