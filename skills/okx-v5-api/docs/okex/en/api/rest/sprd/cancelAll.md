---
site: okex
lang: en
name: Cancel All orders
method: POST
path: /api/v5/sprd/mass-cancel
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel All orders

Cancel all pending orders.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/sprd/mass-cancel`

> Request Example

```shell
POST /api/v5/sprd/mass-cancel
body
{
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
}
```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# cancel all
result = spreadAPI.cancel_all_orders(sprdId="BTC-USDT_BTC-USDT-SWAP")
print(result)
```

#### Request Parameters
| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| sprdId    | String | No       | spread ID   |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "result":true
        }
    ]
}
```

#### Response Example
| Parameter | Type    | Description                       |
|-----------|---------|-----------------------------------|
| result    | Boolean | Result of the request `true`, `false` |

<aside class="notice">
Getting a response with result=true means your request has been successfully received and will be processed. The result of the cancellation is subject to the state pushed by the order channel or the get order state.
</aside>
