---
site: okex
lang: en
name: GET / Purchase&Redeem history
method: GET
path: /api/v5/finance/staking-defi/sol/purchase-redeem-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Purchase&Redeem history

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/sol/purchase-redeem-history`

> Request Example

```shell
GET /api/v5/finance/staking-defi/sol/purchase-redeem-history
```

```python
import okx.Finance.SolStaking as SolStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = SolStaking.SolStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.sol_purchase_redeem_history()
print(result)
```

#### Request Parameters

| Parameter      | Type   | Required | Description                                                                                                            |
|:---------------|:-------|:---------|:-----------------------------------------------------------------------------------------------------------------------|
| type           | String | No       | Type<br>`purchase`<br>`redeem`                                                                                         |
| status         | String | No       | Status<br>`pending`<br>`success`<br>`failed`                                                                          |
| after          | String | No       | Pagination of data to return records earlier than the `requestTime`. The value passed is the corresponding `timestamp` |
| before         | String | No       | Pagination of data to return records newer than the `requestTime`. The value passed is the corresponding `timestamp` |
| limit          | String | No       | Number of results per request. The default is `100`. The maximum is `100`.                                             |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0.62666630",
            "completedTime": "1683413171000",
            "estCompletedTime": "",
            "redeemingAmt": "",
            "requestTime": "1683413171000",
            "status": "success",
            "type": "purchase"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter            | Type   | Description                                                                                          |
|----------------------|--------|------------------------------------------------------------------------------------------------------|
| type                 | String | Type<br>`purchase`<br>`redeem`                                                                       |
| amt                  | String | Purchase/Redeem amount                                                                               |
| redeemingAmt         | String | Redeeming amount                                             |
| status               | String | Status<br>`pending`<br>`success`<br>`failed`                                          |
| requestTime          | String | Request time of make purchase/redeem, Unix timestamp format in milliseconds, e.g. `1597026383085`    |
| completedTime        | String | Completed time of redeem settlement, Unix timestamp format in milliseconds, e.g. `1597026383085`     |
| estCompletedTime     | String | Estimated completed time of redeem settlement, Unix timestamp format in milliseconds, e.g. `1597026383085` |
