---
site: okex
lang: en
name: POST / Purchase
method: POST
path: /api/v5/finance/staking-defi/eth/purchase
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Purchase

Staking ETH for BETH<br>
Only the assets in the funding account can be used.<br>

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/eth/purchase`

> Request Example

```shell
POST /api/v5/finance/staking-defi/eth/purchase
body 
{
    "amt":"100"
}
```

```python
import okx.Finance.EthStaking as EthStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_purchase(amt="1")
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description        |
|:----------|:-------|:------------|:-------------------|
| amt       | String | Yes         | Investment amount  |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
  ]
}
```

#### Response Parameters

code = `0` means your request has been successfully handled.
