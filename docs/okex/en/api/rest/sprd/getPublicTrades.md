---
site: okex
lang: en
name: Get public trades (Public)
method: GET
path: /api/v5/sprd/public-trades
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get public trades (Public)

Retrieve the recent transactions of an instrument (at most 500 records per request). Results are returned in counter chronological order. 

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/sprd/public-trades`

> Request Example

```shell
GET /api/v5/sprd/public-trades?sprdId=BTC-USDT_BTC-USDT-SWAP

```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# get public trades
result = spreadAPI.get_public_trades(sprdId='ETH-USDT-SWAP_ETH-USDT-230929')
print(result)
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| sprdId | String | No | Spread ID, e.g. BTC-USDT_BTC-USDT-SWAP |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "sprdId": "BTC-USDT_BTC-USDC-SWAP",
            "side": "sell",
            "sz": "0.1",
            "px": "964.1",
            "tradeId": "242720719",
            "ts": "1654161641568"
        }
    ]
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| sprdId | String | spread ID |
| tradeId | String | Trade ID |
| px | String | Trade price |
| sz | String | Trade quantity |
| side | String | Trade side of the taker. <br>`buy` <br>`sell` |
| ts | String | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |
