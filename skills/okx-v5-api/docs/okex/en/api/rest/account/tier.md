---
site: okex
lang: en
name: Get PM position limitation
method: GET
path: /api/v5/account/position-tiers
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get PM position limitation

Retrieve cross position limitation of SWAP/FUTURES/OPTION under Portfolio margin mode.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/position-tiers`

> Request Example

```shell
# Query limitation of BTC-USDT
GET /api/v5/account/position-tiers?instType=SWAP&uly=BTC-USDT

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get PM position limitation
result = accountAPI.get_account_position_tiers(
    instType="SWAP",
    uly="BTC-USDT"
)
print(result)
```

#### Request Parameters

| **Parameter**    | **Type** | **Required** | **Description**                                                                                                                                                                  |
|:-----------------| :------- |:-------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType         | String   | Yes          | Instrument type<br>`SWAP`<br>`FUTURES`<br>`OPTION`                                                                                                                           |
| instFamily       | String   | Yes          | Single instrument family or instrument families (no more than 5) separated with comma. |

> Response Example

```json
{
  "code": "0",
  "data": [
    {
      "instFamily": "BTC-USDT",
      "maxSz": "10000",
      "posType": "",
      "uly": "BTC-USDT"
    }
  ],
  "msg": ""
}
```

#### Response Parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:---------------| :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uly            | String   | Underlying<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| instFamily     | String   | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| maxSz          | String   | Max number of positions                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| posType        | String   | Limitation of position type, only applicable to cross `OPTION` under portfolio margin mode <br>`1`: Contracts of pending orders and open positions for all derivatives instruments. `2`: Contracts of pending orders for all derivatives instruments. `3`: Pending orders for all derivatives instruments. `4`: Contracts of pending orders and open positions for all derivatives instruments on the same side. `5`: Pending orders for one derivatives instrument. `6`: Contracts of pending orders and open positions for one derivatives instrument. `7`: Contracts of one pending order. |
