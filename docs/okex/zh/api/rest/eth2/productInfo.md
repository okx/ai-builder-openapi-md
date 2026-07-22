---
site: okex
lang: zh
name: GET / 获取产品信息
method: GET
path: /api/v5/finance/staking-defi/eth/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取产品信息 [financial-product-eth-staking-get-product-info]

#### 限速：3 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/eth/product-info`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/eth/product-info

```

```python
import okx.Finance.EthStaking as EthStaking

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_product_info()
print(result)
```

> 返回结果

```json
{
    "code": "0",
    "data": [
      {
        "fastRedemptionDailyLimit": "100",
        "rate": "2.23",
        "redemptDays": "8",
        "minAmt": "0.001"
      }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                              | 类型     | 描述             |
|----------------------------------|--------|----------------|
| fastRedemptionDailyLimit         | String | 快速赎回每日最高份额<br>母账户和子账户共享同一个限额 |
| rate                             | String | 最新 BETH 年化收益率  |
| redemptDays                      | String | BETH 赎回天数      |
| minAmt                           | String | BETH 最低申购数量    |
