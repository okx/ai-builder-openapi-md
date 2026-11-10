---
site: my
lang: en
name: POST / Cancel purchases/redemptions
method: POST
path: /api/v5/finance/staking-defi/cancel
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### POST / Cancel purchases/redemptions

<aside class="notice">
After cancelling, returning funds will go to the funding account.
</aside>

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/cancel`

> Request Example

```shell
POST /api/v5/finance/staking-defi/cancel
body 
{
    "ordId":"754147",
    "protocolType":"defi"
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

result = StakingAPI.cancel(
           ordId = "1234",
           protocolType = "defi"
        )
print(result)
```

#### Request Parameters

| Parameter    | Type   | Required | Description                                      |
|:-------------|:-------|:---------|:-------------------------------------------------|
| ordId        | String | Yes      | Order ID                                         |
| protocolType | String | Yes      | Protocol type<br>`defi`: on-chain earn |

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
