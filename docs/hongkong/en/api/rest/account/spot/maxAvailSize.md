---
site: hongkong
lang: en
name: Get maximum available balance/equity
method: GET
path: /api/v5/account/max-avail-size
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get maximum available balance/equity

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-avail-size`

> Request Example

```shell
# Query maximum available transaction amount for SPOT BTC-USDT
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cash

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get maximum available transaction amount for SPOT BTC-USDT
result = accountAPI.get_max_avail_size(
    instId="BTC-USDT",
    tdMode="cash"
)
print(result)
```

#### Request Parameters

| **Parameter** | **Type** | Required | Description                                                                                   |
|:--------------| :------- | :------- | :-------------------------------------------------------------------------------------------- |
| instId        | String   | Yes      | Single instrument or multiple instruments (no more than 5) separated with comma, e.g. `BTC-USDT,ETH-USDT`                                                        |
| tdMode        | String   | Yes      | Trade mode<br>`cash`                                                       |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USDT",
      "availBuy": "100",
      "availSell": "1"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| instId        | String   | Instrument ID            |
| availBuy      | String   | Amount available to buy  |
| availSell     | String   | Amount available to sell |

<aside class="notice">
In the case of SPOT, availBuy is in the quote currency, and availSell is in the base currency.<br>
</aside>
