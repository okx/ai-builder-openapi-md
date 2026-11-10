---
site: us
lang: en
name: Get maximum available balance/equity
method: GET
path: /api/v5/account/max-avail-size
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get maximum available balance/equity

Available balance for isolated margin positions and SPOT, available equity for cross margin positions.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-avail-size`

> Request Example

```shell
# Query maximum available transaction amount when cross MARGIN BTC-USDT use BTC as margin
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cross&ccy=BTC

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

| **Parameter** | **Type** | Required | Description                                                                                                                                                                                                                                                               |
|:--------------| :------- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String   | Yes      | Single instrument or multiple instruments (no more than 5) separated with comma, e.g. `BTC-USDT,ETH-USDT`                                                                                                                                                                 |
| ccy           | String   | Conditional | Currency used for margin<br>Applicable to `isolated` `MARGIN` and `cross` `MARGIN` in `Futures mode`.                                                                                                                                                                               |
| tdMode        | String   | Yes      | Trade mode<br>`cross`<br>`isolated`<br>`cash`<br>`spot_isolated`: only applicable to `Futures mode`                                                                                                                                                                                                              |
| reduceOnly    | Boolean  | No       | Whether to reduce position only <br>Only applicable to `MARGIN`                                                                                                                                                                                                           |
| px            | String | No       | The price of closing position. <br>Only applicable to reduceOnly `MARGIN`.                                                                                                                                                                                                |
| tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |

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
| availBuy      | String   | Maximum available balance/equity to buy  |
| availSell     | String   | Maximum available balance/equity to sell |

<aside class="notice">
In the case of SPOT/MARGIN, availBuy is in the quote currency, and availSell is in the base currency.<br>
In the case of MARGIN with cross tdMode, both availBuy and availSell are in the currency passed in <strong>ccy</strong>.
</aside>
