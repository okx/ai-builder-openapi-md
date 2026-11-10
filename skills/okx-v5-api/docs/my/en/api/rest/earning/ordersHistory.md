---
site: my
lang: en
name: GET / Order history
method: GET
path: /api/v5/finance/staking-defi/orders-history
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / Order history

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/orders-history`

> Request Example

```shell
GET /api/v5/finance/staking-defi/orders-history

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_orders_history()
print(result)
```

#### Request Parameters

| Parameter    | Type   | Required | Description                                                                                                       |
|:-------------|:-------|:---------|:------------------------------------------------------------------------------------------------------------------|
| productId    | String | No       | Product ID                                                                                                        |
| protocolType | String | No       | Protocol type<br>`defi`: on-chain earn                                                                  |
| ccy          | String | No       | Investment currency, e.g. `BTC`                                                                                   |
| after        | String | No       | Pagination of data to return records earlier than the requested ID. The value passed is the corresponding `ordId` |
| before       | String | No       | Pagination of data to return records newer than the requested ID. The value passed is the corresponding `ordId`   |
| limit        | String | No       | Number of results per request. The default is `100`. The maximum is `100`.                                        |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
       {
            "ordId": "1579252",
            "ccy": "DOT",
            "productId": "101",
            "state": "3",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1704",
            "investData": [
                {
                    "ccy": "DOT",
                    "amt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0",
                    "realizedEarnings": "0"
                }
            ],
            "purchasedTime": "1712908001000",
            "redeemedTime": "1712914294000",
            "tag": ""
       }
    ]
}
```

#### Response Parameters

| Parameter          | Type   | Description                                                                                                  |
|--------------------|--------|--------------------------------------------------------------------------------------------------------------|
| ccy                | String | Currency, e.g. `BTC`                                                                                         |
| ordId              | String | Order ID                                                                                                     |
| productId          | String | Product ID                                                                                                   |
| state              | String | Order state<br>`3`: Completed (including canceled and redeemed)                                              |
| protocol           | String | Protocol                                                                                                     |
| protocolType       | String | Protocol type<br>`defi`: on-chain earn                                                                       |
| term               | String | Protocol term<br>It will return the days of fixed term and will return `0` for flexible product              |
| apy                | String | Estimated APY<br>If the estimated APY is 7% , this field is `0.07`<br>Retain to 4 decimal places (truncated) |
| investData         | Array of objects  | Investment data                                                                                              |
| > ccy              | String | Investment currency, e.g. `BTC`                                                                              |
| > amt              | String | Invested amount                                                                                              |
| earningData        | Array of objects  | Earning data                                                                                                 |
| > ccy              | String | Earning currency, e.g. `BTC`                                                                                 |
| > earningType      | String | Earning type<br>`0`: Estimated earning<br>`1`: Cumulative earning                                            |
| > realizedEarnings | String | Cumulative earning of redeemed orders<br>This field is just valid when the order is in redemption state      |
| purchasedTime      | String | Order purchased time, Unix timestamp format in milliseconds, e.g. `1597026383085`                            |
| redeemedTime       | String | Order redeemed time, Unix timestamp format in milliseconds, e.g. `1597026383085`                             |
| tag                | String | Order tag                                                                                                    |
