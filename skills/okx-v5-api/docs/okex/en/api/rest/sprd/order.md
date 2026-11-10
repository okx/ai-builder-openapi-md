---
site: okex
lang: en
name: Place order
method: POST
path: /api/v5/sprd/order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Place order

Place a new order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/sprd/order`

> Request Example

```shell
# place order for a spread
POST /api/v5/sprd/order
body
{
   "sprdId":"BTC-USDT_BTC-USDT-SWAP",
   "clOrdId":"b15",
   "side":"buy",
   "ordType":"limit",
   "px":"2.15",
   "sz":"2"
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

# place order
result = spreadAPI.place_order(sprdId='BTC-USDT_BTC-USDT-SWAP',
                               clOrdId='b16',side='buy',ordType='limit',
                               px='2',sz='2')
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                                                      |
|-----------|--------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| sprdId    | String | Yes         | spread ID, e.g. BTC-USDT_BTC-USD-SWAP                                                                                                            |
| clOrdId   | String | No          | Client Order ID as assigned by the client <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag       | String | No          | Order tag <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                 |
| side      | String | Yes         | Order side, `buy` `sell`                                                                                                                             |
| ordType   | String | Yes         | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order<br>`ioc`: Immediate-or-cancel order                                                                                                                 |
| sz        | String | Yes         | Quantity to buy or sell. The unit is USD for inverse spreads, and the corresponding baseCcy for linear and hybrid spreads.                                                                                                                          |
| px        | String | Yes | Order price. Only applicable to `limit`, `post_only`, `ioc`                                                                                   |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "clOrdId": "b15",
      "ordId": "312269865356374016",
      "tag": "",
      "sCode": "0",
      "sMsg": ""
    }
  ]
}
```

#### Response Example

| Parameter | Type   | Description                                              |
|-----------|--------|----------------------------------------------------------|
| ordId     | String | Order ID                                                 |
| clOrdId   | String | Client Order ID as assigned by the client                |
| tag       | String | Order tag                                                |
| sCode     | String | The code of the event execution result, 0 means success. |
| sMsg      | String | Rejection or success message of event execution.         |

<aside class="notice">
clOrdId <br>
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders.
clOrdId must be unique among the clOrdIds of all pending orders.
</aside>
<aside class="notice">
ordType <br>
Order type. When creating a new order, you must specify the order type. The order type you specify will affect: 1) what order parameters are required, and 2) how the matching system executes your order. The following are valid order types:<br>
limit: Limit order, which requires specified sz and px.<br>
post_only: Post-only order, which the order can only provide liquidity to the market and be a maker. If the order would have executed on placement, it will be canceled instead.
ioc: Immediate-or-cancel order
</aside>
<aside class="notice">
sz <br>
The sz unit for inverse spreads is USD in Nitro Spread, as opposed to contract in OKX orderbook.
</aside>
