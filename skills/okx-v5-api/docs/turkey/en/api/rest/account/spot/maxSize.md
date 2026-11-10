---
site: turkey
lang: en
name: Get maximum order quantity
method: GET
path: /api/v5/account/max-size
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get maximum order quantity

The maximum quantity to buy or sell. It corresponds to the "sz" from placement.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/max-size`

> Request Example

```shell
GET /api/v5/account/max-size?instId=BTC-USDT&tdMode=cash

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
    tdMode="cash"
)
print(result)
```

#### Request Parameters

| **Parameter** | **Type** | Required | Description                                                                                                                                                                                                                                              |
| :------------ | :------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String   | Yes      | Single instrument or multiple instruments (no more than 5) in the same instrument type separated with comma, e.g. `BTC-USDT,ETH-USDT`                                                                                                                                                |
| tdMode        | String   | Yes      | Trade mode<br>`cash`                                                                                                                                                                                                                  |
| px            | String   | No       | Price<br>The parameter will be ignored when multiple instruments are specified.                                                                                                                                                                          |

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

| **Parameter** | **Type** | **Description**                                                     |
| :------------ | :------- |:--------------------------------------------------------------------|
| instId        | String   | Instrument ID                                                       |
| ccy           | String   | Currency used for margin                                            |
| maxBuy        | String   | `SPOT`: The maximum quantity in base currency that you can buy      |
| maxSell       | String   | `SPOT`: The maximum quantity in quote currency that you can sell    |
