---
site: my
lang: en
name: Get the maximum loan of instrument
method: GET
path: /api/v5/account/max-loan
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get the maximum loan of instrument

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-loan`

> Request Example

```shell
# Max loan of cross `MARGIN` for currencies of trading pair in `Spot mode` (enabled borrowing)
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross

# Max loan for currency in `Spot mode` (enabled borrowing)
GET  /api/v5/account/max-loan?ccy=USDT&mgnMode=cross

# Max loan of isolated `MARGIN` in `Futures mode`
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=isolated

# Max loan of cross `MARGIN` in `Futures mode` (Margin Currency is BTC)
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross&mgnCcy=BTC

# Max loan of cross `MARGIN` in `Multi-currency margin`
GET  /api/v5/account/max-loan?instId=BTC-USDT&mgnMode=cross

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Max loan of cross MARGIN in Futures mode (Margin Currency is BTC)
result = accountAPI.get_max_loan(
    instId="BTC-USDT",
    mgnMode="cross",
    mgnCcy="BTC"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                 |
| :-------- | :----- |:------------|:------------------------------------------------------------------------------------------------------------|
| mgnMode   | String | Yes         |  Margin mode<br>`isolated` `cross`                                                                          |
| instId    | String | Conditional | Single instrument or multiple instruments (no more than 5) separated with comma, e.g. `BTC-USDT,ETH-USDT`   |
| ccy       | String | Conditional | Currency<br>Applicable to get Max loan of manual borrow for the currency in `Spot mode` (enabled borrowing) |
| mgnCcy    | String | Conditional | Margin currency<br>Applicable to `isolated` `MARGIN` and `cross` `MARGIN` in `Futures mode`.                             |
| tradeQuoteCcy | String | No       | The quote currency for trading. Only applicable to `SPOT`.<br>The default value is the quote currency of `instId`, e.g. `USD` for `BTC-USD`. |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USDT",
      "mgnMode": "isolated",
      "mgnCcy": "",
      "maxLoan": "0.1",
      "ccy": "BTC",
      "side": "sell"
    },
    {
      "instId": "BTC-USDT",
      "mgnMode": "isolated",
      "mgnCcy": "",
      "maxLoan": "0.2",
      "ccy": "USDT",
      "side": "buy"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                    |
| :------------ | :------- | :------------------------------------------------------------------------------------------------- |
| instId        | String   | Instrument ID                                                                                      |
| mgnMode       | String   | Margin mode                                                                                        |
| mgnCcy        | String   | Margin currency |
| maxLoan       | String   | Max loan                                                                                           |
| ccy           | String   | Currency                                                                                           |
| side          | String   | Order side<br>`buy` `sell`                                                                         |
