---
site: okex
lang: en
name: POST / Purchase
method: POST
path: /api/v5/finance/staking-defi/purchase
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Purchase

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/purchase`

> Request Example

```shell
# Invest 100ZIL 30-day staking protocol
POST /api/v5/finance/staking-defi/purchase
body 
{
    "productId":"1234",
    "investData":[
      {
        "ccy":"ZIL",
        "amt":"100"
      }
    ],
    "term":"30"
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

result = StakingAPI.purchase(
            productId = "4005", 
            investData = [{
                "ccy":"USDT",
                "amt":"100"
            }]
        )
print(result)

```

#### Request Parameters

| Parameter  | Type   | Required    | Description                                                                                                     |
|:-----------|:-------|:------------|:----------------------------------------------------------------------------------------------------------------|
| productId  | String | Yes         | Product ID                                                                                                      |
| investData | Array of objects  | Yes         | Investment data                                                                                                 |
| > ccy      | String | Yes         | Investment currency, e.g. `BTC`                                                                                 |
| > amt      | String | Yes         | Investment amount                                                                                               |
| term       | String | Conditional | Investment term<br>Investment term must be specified for fixed-term product                                     |
| tag        | String | No          | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |

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
