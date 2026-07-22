---
site: okex
lang: en
name: POST / Redeem
method: POST
path: /api/v5/finance/staking-defi/sol/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Redeem

Only the assets in the funding account can be used. If your OKSOL is in your trading account, you can make funding transfer first.<br>

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/sol/redeem`

> Request Example

```shell
POST /api/v5/finance/staking-defi/sol/redeem
body 
{
    "amt": "10"
}
```

```python
import okx.Finance.SolStaking as SolStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = SolStaking.SolStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.sol_redeem(amt="1")
print(result)
```

#### Request Parameters

| Parameter        | Type    | Required | Description      |
|:-----------------|:--------|:---------|:-----------------|
| amt              | String  | Yes      | Redeeming amount |

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
