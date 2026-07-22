---
site: okex
lang: en
name: Get order book (Public)
method: GET
path: /api/v5/sprd/books
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get order book (Public)

Retrieve the order book of the spread.

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/sprd/books`

> Request Example

```shell
GET /api/v5/sprd/books?sprdId=BTC-USDT_BTC-USDT-SWAP

```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# get order book
result = spreadAPI.get_order_book(sprdId="BTC-USDT_BTC-USDT-SWAP", sz=20)
print(result)
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| sprdId | String | Yes | spread ID, e.g. BTC-USDT_BTC-USDT-SWAP |
| sz | String | No | Order book depth per side. Maximum value is 400. Default value is 5.|

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "41006.8", // price
                    "0.60038921", // quantity
                    "1" // number of orders at the price
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "2"
                ]
            ],
            "ts": "1629966436396"
        }
    ]
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| asks | Array of arrays | Order book on sell side |
| bids | Array of arrays | Order book on buy side |
| ts | String | Order book generation time |

<aside class="notice">
An example of the array of asks and bids values: ["411.8", "10", "4"]<br>
- "411.8" is the depth price<br>
- "10" is the quantity at the price (Unit: szCcy)<br>
- "4" is the number of orders at the price.<br>
</aside>
