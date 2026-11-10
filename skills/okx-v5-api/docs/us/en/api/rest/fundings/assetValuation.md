---
site: us
lang: en
name: Get account asset valuation
method: GET
path: /api/v5/asset/asset-valuation
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get account asset valuation

View account asset valuation

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/asset-valuation`

> Request Example

```shell
GET /api/v5/asset/asset-valuation
```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Get account asset valuation
result = fundingAPI.get_asset_valuation()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy  | String | No       | Asset valuation calculation unit <br>BTC, USDT<br>USD, CNY, JP, KRW, RUB, EUR<br>VND, IDR, INR, PHP, THB, TRY <br>AUD, SGD, ARS, SAR, AED, IQD <br>The default is the valuation in BTC. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "details": {
                "classic": "124.6",
                "earn": "1122.73",
                "funding": "0.09",
                "trading": "2544.28"
            },
            "totalBal": "3790.09",
            "ts": "1637566660769"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                                                         |
| ------ |--------| ------------------------------------------------------------ |
| totalBal       | String | Valuation of total account assets  |
| ts       | String | Unix timestamp format in milliseconds, e.g.`1597026383085`         |
| details | Object | Asset valuation details for each account |
| > funding  | String | Funding account       |
| > trading  | String | Trading account       |
| > classic  | String | [Deprecated] Classic account    |
| > earn     | String | Earn account       |
