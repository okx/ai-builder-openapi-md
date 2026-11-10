---
site: okex
lang: en
name: GET / Product info
method: GET
path: /api/v5/finance/staking-defi/eth/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Product info

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/eth/product-info`

> Request Example

```shell
GET /api/v5/finance/staking-defi/eth/product-info

```

```python
import okx.Finance.EthStaking as EthStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_product_info()
print(result)
```

> Response Example

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

#### Response Parameters

| Parameter                       | Type   | Description                                                                             |
|---------------------------------|--------|-----------------------------------------------------------------------------------------|
| fastRedemptionDailyLimit        | String | Fast redemption daily limit<br>The master account and sub-accounts share the same limit |
| rate                            | String | Latest BETH APY                                                                         |
| redemptDays                     | String | Redemption days of BETH                                                                 |
| minAmt                          | String | Minimum subscription amount of BETH                                                     |
