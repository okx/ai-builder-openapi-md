---
site: okex
lang: en
name: Get Spreads (Public)
method: GET
path: /api/v5/sprd/spreads
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get Spreads (Public)

Retrieve all available spreads based on the request parameters.

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/sprd/spreads`

> Request Example

```shell
GET /api/v5/sprd/spreads?instId=BTC-USDT

```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# get spreads
result = spreadAPI.get_spreads()
print(result)
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| baseCcy | string | No | Currency instrument is based in, e.g. BTC, ETH |
| instId | String | No | The instrument ID to be included in the spread. |
| sprdId | String | No | The spread ID |
| state | string | No | Spreads which are available to trade, suspened or expired. Valid values include `live`, `suspend` and `expired`. |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"sprdId": "ETH-USD-SWAP_ETH-USD-231229",
			"sprdType": "inverse",
			"state": "live",
			"baseCcy": "ETH",
			"szCcy": "USD",
			"quoteCcy": "USD",
			"tickSz": "0.01",
			"minSz": "10",
			"lotSz": "10",
			"listTime": "1686903000159",
			"legs": [{
					"instId": "ETH-USD-SWAP",
					"side": "sell"
				},
				{
					"instId": "ETH-USD-231229",
					"side": "buy"
				}
			],
			"expTime": "1703836800000",
			"uTime": "1691376905595"
		},
		{
			"sprdId": "BTC-USDT_BTC-USDT-SWAP",
			"sprdType": "linear",
			"state": "live",
			"baseCcy": "BTC",
			"szCcy": "BTC",
			"quoteCcy": "USDT",
			"tickSz": "0.0001",
			"minSz": "0.001",
			"lotSz": "1",
			"listTime": "1597026383085",
			"expTime": "1597029999085",
			"uTime": "1597028888085",
			"legs": [{
					"instId": "BTC-USDT",
					"side": "sell"
				},
				{
					"instId": "BTC-USDT-SWAP",
					"side": "buy"
				}
			]
		},
		{
			"sprdId": "BTC-USDT_BTC-USDT-230317",
			"sprdType": "linear",
			"state": "live",
			"baseCcy": "BTC",
			"szCcy": "BTC",
			"quoteCcy": "USDT",
			"tickSz": "0.0001",
			"minSz": "0.001",
			"lotSz": "1",
			"listTime": "1597026383085",
			"expTime": "1597029999085",
			"uTime": "1597028888085",
			"legs": [{
					"instId": "BTC-USDT",
					"side": "sell"
				},
				{
					"instId": "BTC-USDT-230317",
					"side": "buy"
				}
			]
		}
	]
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| sprdId | String | spread ID |
| sprdType | String | spread Type. Valid values are `linear`, `inverse`, `hybrid`  |
| state | String | Current state of the spread. Valid values include `live`, `expired`, `suspend`. |
| baseCcy | String | Currency instrument is based in. Valid values include BTC, ETH |
| szCcy | String | The currency the spread order size is submitted to the underlying venue in, e.g. USD, BTC, ETH.|
| quoteCcy | String | The currency the spread is priced in, e.g. USDT, USD|
| tickSz | String | Tick size, e.g. 0.0001 in the quoteCcy of the spread. |
| minSz | String | Minimum order size in the szCcy of the spread. |
| lotSz | String | The minimum order size increment the spread can be traded in the szCcy of the spread. |
| listTime | String | The timestamp the spread was created. Unix timestamp format in milliseconds, , e.g. `1597026383085` |
| expTime | String | Expiry time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime | String | The timestamp the spread was last updated. Unix timestamp format in milliseconds, e.g. `1597026383085` |
| legs | array of objects |  |
| > instId | String | Instrument ID, e.g. BTC-USD-SWAP |
| > side | String | The direction of the leg of the spread. Valid Values include `buy` and `sell`. |
