---
site: okex
lang: zh
name: POST / 申购
method: POST
path: /api/v5/finance/staking-defi/eth/purchase
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 申购 [financial-product-eth-staking-post-purchase]

质押ETH获取BETH<br>
仅资金账户中的资产支持ETH质押。

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/staking-defi/eth/purchase`

> 请求示例

```shell
POST /api/v5/finance/staking-defi/eth/purchase
body 
{
    "amt":"100"
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

result = StackingAPI.eth_purchase(amt="1")
print(result)
```

#### 请求参数

| 参数  | 类型     | 是否必须 | 描述          |
|:----|:-------|:-----|:------------|
| amt | String | 是    | 投资数量 |

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
