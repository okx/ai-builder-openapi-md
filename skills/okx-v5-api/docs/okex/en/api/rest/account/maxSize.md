---
site: okex
lang: en
name: Get maximum order quantity
method: GET
path: /api/v5/account/max-size
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get maximum order quantity

The maximum quantity to buy or sell. It corresponds to the "sz" from placement.

<aside class="notice">
Under the Portfolio Margin account, the calculation of the maximum buy/sell amount or open amount is not supported under the cross mode of derivatives.
</aside>

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-size`

> Request Example

```shell
GET /api/v5/account/max-size?instId=BTC-USDT&tdMode=isolated

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get maximum buy/sell amount or open amount
result = accountAPI.get_max_order_size(
    instId="BTC-USDT",
    tdMode="isolated"
)
print(result)
```

#### Request Parameters

| **Parameter** | **Type** | Required | Description                                                                                                                                                                                                                                                |
| :------------ | :------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String   | Yes      | Single instrument or multiple instruments (no more than 5) in the same instrument type separated with comma, e.g. `BTC-USDT,ETH-USDT`                                                                                                                      |
| tdMode        | String   | Yes      | Trade mode<br>`cross`<br>`isolated`<br>`cash`<br>`spot_isolated`: only applicable to `Futures mode`.                                                                                                                                              |
| ccy           | String   | Conditional | Currency used for margin <br>Applicable to `isolated` `MARGIN` and `cross` `MARGIN` orders in `Futures mode`.                                                                                                                                     |
| px            | String   | No       | Price<br>When the price is not specified, it will be calculated according to the current limit price for `FUTURES` and `SWAP`, the last traded price for other instrument types.<br>The parameter will be ignored when multiple instruments are specified. |
| leverage      | String   | No       | Leverage for instrument<br>The default is current leverage<br>Only applicable to `MARGIN/FUTURES/SWAP`                                                                                                                                                     |
| tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |
| outcome | String | No | Market outcome to trade on.<br>`yes`<br>`no`<br>Only applicable and optional for `EVENTS`, the default value is `yes` |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"instId": "BTC-USDT",
		"maxBuy": "0.0500695098559788",
		"maxSell": "64.4798671570072269"
  }]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                          |
| :------------ | :------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| instId        | String   | Instrument ID                                                                                                                            |
| ccy           | String   | Currency used for margin           |
| maxBuy        | String   | `SPOT`/`MARGIN`: The maximum quantity in base currency that you can buy<br>The cross-margin order under `Futures mode` mode, quantity of coins is based on base currency.<br>`FUTURES`/`SWAP`/`OPTIONS`: The maximum quantity of contracts that you can buy   |
| maxSell       | String   | `SPOT`/`MARGIN`: The maximum quantity in quote currency that you can sell<br>The cross-margin order under `Futures mode` mode, quantity of coins is based on base currency.<br>`FUTURES`/`SWAP`/`OPTIONS`: The maximum quantity of contracts that you can sell |
