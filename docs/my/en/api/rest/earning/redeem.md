---
site: my
lang: en
name: POST / Redeem
method: POST
path: /api/v5/finance/staking-defi/redeem
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### POST / Redeem

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/redeem`

> Request Example

```shell
# Early redemption of investment
POST /api/v5/finance/staking-defi/redeem
body 
{
    "ordId":"754147",
    "protocolType":"defi",
    "allowEarlyRedeem":true
}
```

```python
import okx.Finance.StakingDefi as StakingDefi

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.redeem(
           ordId = "1234",
           protocolType = "defi"
        )
print(result)

```

#### Request Parameters

| Parameter        | Type    | Required | Description                                           |
|:-----------------|:--------|:---------|:------------------------------------------------------|
| ordId            | String  | Yes      | Order ID                                              |
| protocolType     | String  | Yes      | Protocol type<br>`defi`: on-chain earn      |
| allowEarlyRedeem | Boolean | No       | Whether allows early redemption<br>Default is `false` |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

#### Response Parameters

| Parameter | Type   | Description |
|-----------|--------|-------------|
| ordId     | String | Order ID    |
| tag       | String | Order tag   |
