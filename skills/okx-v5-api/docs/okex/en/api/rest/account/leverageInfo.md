---
site: okex
lang: en
name: Get leverage
method: GET
path: /api/v5/account/leverage-info
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get leverage

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/leverage-info`

> Request Example

```shell
GET /api/v5/account/leverage-info?instId=BTC-USDT-SWAP&mgnMode=cross

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get leverage
result = accountAPI.get_leverage(
    instId="BTC-USDT-SWAP",
    mgnMode="cross"
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required       | Description                                                                                                                                                                                                                                    |
| :------ | :----- |:---------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId  | String | Conditional    | Instrument ID<br>Single instrument ID or multiple instrument IDs (no more than 20) separated with comma                                                                                                                                        |
| ccy     | String | Conditional    | Currency，used for getting leverage of currency level.<br>Applicable to `cross` `MARGIN` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`.<br>Supported single currency or multiple currencies (no more than 20) separated with comma. |
| mgnMode | String | Yes            | Margin mode<br>`cross` `isolated`                                                                                                                                                                                                              |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
        "ccy":"",
		"instId": "BTC-USDT-SWAP",
		"mgnMode": "cross",
		"posSide": "long",
		"lever": "10"
	},{
        "ccy":"",
		"instId": "BTC-USDT-SWAP",
		"mgnMode": "cross",
		"posSide": "short",
		"lever": "10"
	}]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                    |
|:--------------| :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String   | Instrument ID                                                                                                                                      |
| ccy           | String | Currency，used for getting leverage of currency level.<br>Applicable to `cross` `MARGIN` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`. |
| mgnMode       | String   | Margin mode                                                                                                                                        |
| posSide       | String   | Position side<br>`long` <br>`short` <br>`net`<br>In `long/short` mode, the leverage in both directions `long`/`short` will be returned.            |
| lever         | String   | Leverage                                                                                                                                           |

<aside class="notice">
Leverage cannot be enquired for the cross positions of Expiry Futures and Perpetual Futures under the portfolio margin account.
</aside>
