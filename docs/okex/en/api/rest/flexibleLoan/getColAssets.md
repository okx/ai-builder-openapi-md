---
site: okex
lang: en
name: GET / Collateral assets
method: GET
path: /api/v5/finance/flexible-loan/collateral-assets
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Collateral assets

Get collateral assets in funding account.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/flexible-loan/collateral-assets`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/collateral-assets?ordId=12345
```

```python
from okx.Finance import FlexibleLoan

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.collateral_assets(ordId="12345")
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                 |
| :------------- | :-------- | :----------- |:--------------------------------|
| ccy            | String    | No           | Collateral currency, e.g. `BTC` |
| ordId        | String.   | No           | Order ID of your flexible loan.<br>If `ordId` is not passed, system will assume it is acting against the existing order with the earliest order start time.<br>If there are no existing orders, system will return empty result data. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "assets": [
                {
                    "amt": "1.7921483143067599",
                    "ccy": "BTC",
                    "notionalUsd": "158292.621793314105231"
                },
                {
                    "amt": "1.9400755578876945",
                    "ccy": "ETH",
                    "notionalUsd": "6325.6652712507628946"
                },
                {
                    "amt": "63.9795959720319628",
                    "ccy": "USDT",
                    "notionalUsd": "64.3650372635940345"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type**        | **Description**           |
|:--------------|:----------------|:--------------------------|
| assets        | Array of objects | Collateral assets data    |
| > ccy         | String          | Currency, e.g. `BTC`      |
| > amt         | String          | Available amount          |
| > notionalUsd | String          | Notional value in `USD`     |
