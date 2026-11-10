---
site: okex
lang: en
name: GET / Active orders
method: GET
path: /api/v5/finance/staking-defi/orders-active
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Active orders 

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/orders-active`

> Request Example

```shell
GET /api/v5/finance/staking-defi/orders-active

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_activity_orders()
print(result)
```

#### Request Parameters

| Parameter     | Type    | Required | Description                                                                                           |
| :------------ | :-----  | :------- |:------------------------------------------------------------------------------------------------------|
| productId     | String  | No       | Product ID                                                                                            |
| protocolType  | String  | No       | Protocol type<br>`defi`: on-chain earn                                                                |
| ccy           | String  | No       | Investment currency, e.g. `BTC`                                                                       |
| state         | String  | No       | Order state<br>`8`: Pending <br>`13`: Cancelling <br>`9`: Onchain <br>`1`: Earning <br>`2`: Redeeming |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "2413499",
            "ccy": "DOT",
            "productId": "101",
            "state": "1",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1014",
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
                    "earnings": "0.10615025"
                }
            ],
            "purchasedTime": "1729839328000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        },
        {
            "ordId": "2213257",
            "ccy": "USDT",
            "productId": "4005",
            "state": "1",
            "protocol": "On-Chain Defi",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.0323",
            "investData": [
                {
                    "ccy": "USDT",
                    "amt": "1"
                }
            ],
            "earningData": [
                {
                    "ccy": "USDT",
                    "earningType": "0",
                    "earnings": "0.02886582"
                },
                {
                    "ccy": "COMP",
                    "earningType": "1",
                    "earnings": "0.0000627"
                }
            ],
            "purchasedTime": "1725345790000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        },
        {
            "ordId": "2210943",
            "ccy": "USDT",
            "productId": "4005",
            "state": "1",
            "protocol": "On-Chain Defi",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.0323",
            "investData": [
                {
                    "ccy": "USDT",
                    "amt": "1"
                }
            ],
            "earningData": [
                {
                    "ccy": "USDT",
                    "earningType": "0",
                    "earnings": "0.02891823"
                },
                {
                    "ccy": "COMP",
                    "earningType": "1",
                    "earnings": "0.0000632"
                }
            ],
            "purchasedTime": "1725280801000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter                | Type            | Description                                                                                                |
|--------------------------|-----------------|------------------------------------------------------------------------------------------------------------|
| ccy                      | String          | Currency, e.g. `BTC`                                                                                       |
| ordId                    | String          | Order ID                                                                                                   |
| productId                | String          | Product ID                                                                                                 |
| state                    | String          | Order state<br>`8`: Pending <br>`13`: Cancelling <br>`9`: Onchain <br>`1`: Earning <br>`2`: Redeeming                |
| protocol                 | String          | Protocol                                                                                                   |
| protocolType             | String          | Protocol type<br>`defi`: on-chain earn                                     |
| term                     | String          | Protocol term<br>It will return the days of fixed term and will return `0` for flexible product                              |                                        
| apy                      | String          | Estimated APY<br>If the estimated APY is 7% , this field is 0.07<br>Retain to 4 decimal places (truncated) |
| investData               | Array of objects | Investment data                                                                                            |
| > ccy                    | String          | Investment currency, e.g. `BTC`                                                                            |
| > amt                    | String          | Invested amount                                                                                            |
| earningData              | Array of objects | Earning data                                                                                               |
| > ccy                    | String          | Earning currency, e.g. `BTC`                                                                               |
| > earningType            | String          | Earning type<br>`0`: Estimated earning<br>`1`: Cumulative earning                                          |
| > earnings               | String          | Earning amount                                                                                             |
| fastRedemptionData       | Array of objects | Fast redemption data                                                                                               |
| > ccy                    | String          | Currency, e.g. `BTC`                                                                               |
| > redeemingAmt            | String          | Redeeming amount                                          |
| purchasedTime            | String          | Order purchased time, Unix timestamp format in milliseconds, e.g. `1597026383085`                          |
| estSettlementTime        | String          | Estimated redemption settlement time                                                                       |
| cancelRedemptionDeadline | String          | Deadline for cancellation of redemption application                                                        |
| tag                      | String          | Order tag                                                                                                  |
