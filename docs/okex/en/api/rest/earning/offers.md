---
site: okex
lang: en
name: GET / Offers
method: GET
path: /api/v5/finance/staking-defi/offers
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Offers 

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/offers`

> Request Example

```shell
GET /api/v5/finance/staking-defi/offers

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_offers(ccy="USDT")
print(result)

```

#### Request Parameters

| Parameter    | Type   | Required | Description                                      |
|:-------------|:-------|:---------|:-------------------------------------------------|
| productId    | String | No       | Product ID                                       |
| protocolType | String | No       | Protocol type<br>`defi`: on-chain earn |
| ccy          | String | No       | Investment currency, e.g. `BTC`                  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "DOT",
            "productId": "101",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1767",
            "earlyRedeem": false,
            "state": "purchasable",
            "investData": [
                {
                    "bal": "0",
                    "ccy": "DOT",
                    "maxAmt": "0",
                    "minAmt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0"
                }
            ],
            "fastRedemptionDailyLimit": "",
            "redeemPeriod": [
                "28D",
                "28D"
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter                | Type            | Description                                                                                                                                                                                                         |
|--------------------------|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                      | String          | Currency type, e.g. `BTC`                                                                                                                                                                                           |
| productId                | String          | Product ID                                                                                                                                                                                                          |
| protocol                 | String          | Protocol                                                                                                                                                                                                            |
| protocolType             | String          | Protocol type<br>`defi`: on-chain earn                                                                                                                                                                              |
| term                     | String          | Protocol term<br>It will return the days of fixed term and will return `0` for flexible product                                                                                                                     |
| apy                      | String          | Estimated annualization<br>If the annualization is 7% , this field is 0.07                                                                                                                                          |
| earlyRedeem              | Boolean         | Whether the protocol supports early redemption                                                                                                                                                                      |
| investData               | Array of objects | Current target currency information available for investment                                                                                                                                                        |
| > ccy                    | String          | Investment currency, e.g. `BTC`                                                                                                                                                                                     |
| > bal                    | String          | Available balance to invest                                                                                                                                                                                         |
| > minAmt                 | String          | Minimum subscription amount                                                                                                                                                                                         |
| > maxAmt                 | String          | Maximum available subscription amount                                                                                                                                                                               |
| earningData              | Array of objects | Earning data                                                                                                                                                                                                        |
| > ccy                    | String          | Earning currency, e.g. `BTC`                                                                                                                                                                                        |
| > earningType            | String          | Earning type<br>`0`: Estimated earning<br>`1`: Cumulative earning                                                                                                                                                   |
| state                    | String          | Product state<br>`purchasable`: Purchasable<br>`sold_out`: Sold out<br>`Stop`: Suspension of subscription                                                                                                           |
| redeemPeriod             | Array of strings | Redemption Period, format in [min time,max time]<br>`H`: Hour, `D`: Day<br>e.g. ["1H","24H"] represents redemption period is between 1 Hour and 24 Hours.<br>["14D","14D"] represents redemption period is 14 days. |
| fastRedemptionDailyLimit | String          | Fast redemption daily limit<br>If fast redemption is not supported, it will return ''.                                                                                                                                                                                     |
