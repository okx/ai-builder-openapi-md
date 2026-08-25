---
site: okex
lang: en
name: Get non-tradable assets
method: GET
path: /api/v5/asset/non-tradable-assets
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get non-tradable assets

Retrieve the funding account balances of all the assets and the amount that is available or on hold.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/non-tradable-assets`

> Request Example

```shell
GET /api/v5/asset/non-tradable-assets

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

result = fundingAPI.get_non_tradable_assets()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                              |
| :------------- | :-------- | :----------- | :----------------------------------------------------------- |
| ccy            | String    | No           | Single currency or multiple currencies (no more than 20) separated with comma, e.g. `BTC` or `BTC,ETH`. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "bal": "989.84719571",
            "burningFeeRate": "",
            "canWd": true,
            "ccy": "CELT",
            "chain": "CELT-OKTC",
            "ctAddr": "f403fb",
            "fee": "2",
            "feeCcy": "USDT",
            "logoLink": "https://static.coinall.ltd/cdn/assets/imgs/221/460DA8A592400393.png",
            "minWd": "0.1",
            "name": "",
            "needTag": false,
            "wdAll": false,
            "wdTickSz": "8"
        },
        {
            "bal": "0.001",
            "burningFeeRate": "",
            "canWd": true,
            "ccy": "MEME",
            "chain": "MEME-ERC20",
            "ctAddr": "09b760",
            "fee": "5",
            "feeCcy": "USDT",
            "logoLink": "https://static.coinall.ltd/cdn/assets/imgs/207/2E664E470103C613.png",
            "minWd": "0.001",
            "name": "MEME Inu",
            "needTag": false,
            "wdAll": false,
            "wdTickSz": "8"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter      | Type    | Description                                                                                                                                                                                                       |
|:---------------|:--------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy            | String  | Currency, e.g. `CELT`                                                                                                                                                                                             |
| name           | String  | Chinese name of currency. There is no related name when it is not shown.                                                                                                                                          |
| logoLink       | String  | Logo link of currency                                                                                                                                                                                             |
| bal            | String  | Withdrawable balance                                                                                                                                                                                              |
| canWd          | Boolean | Availability to withdraw to chain. <br>`false`: not available  `true`: available                                                                                                                                  |
| chain          | String  | Chain for withdrawal                                                                                                                                                                                              |
| minWd          | String  | Minimum withdrawal amount of currency in a single transaction                                                                                                                                                     |
| wdAll          | Boolean | Whether all assets in this currency must be withdrawn at one time                                                                                                                                                 |
| fee            | String  | Fixed withdrawal fee                                                                                                                                                                                              |
| feeCcy          | String  | Fixed withdrawal fee unit, e.g. `USDT`                                                                                                                                                                            |
| burningFeeRate  | String  | Burning fee rate, e.g "0.05" represents "5%".<br>Some currencies may charge combustion fees. The burning fee is deducted based on the withdrawal quantity (excluding gas fee) multiplied by the burning fee rate. |
| ctAddr         | String  | Last 6 digits of contract address                                                                                                                                                                                 |
| wdTickSz       | String  | Withdrawal precision, indicating the number of digits after the decimal point                                                                                                                                     |
| needTag        | Boolean | Whether tag/memo information is required for withdrawal                                                                                                                                                           |
