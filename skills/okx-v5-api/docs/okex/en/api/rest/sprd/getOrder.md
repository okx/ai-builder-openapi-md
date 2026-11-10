---
site: okex
lang: en
name: Get order details
method: GET
path: /api/v5/sprd/order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get order details

Retrieve order details.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/sprd/order`

> Request Example

```shell
GET /api/v5/sprd/order?ordId=2510789768709120
```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# get order details
result = spreadAPI.get_order_details(ordId='1905309079888199680')
print(result)
```

#### Request Parameters
| Parameter | Type   | Required    | Description                                                                              |
|-----------|--------|-------------|------------------------------------------------------------------------------------------|
| ordId     | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used |
| clOrdId   | String | Conditional | Client Order ID as assigned by the client. The latest order will be returned.            |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "instId": "BTC-USD-200329",
      "ordId": "312269865356374016",
      "clOrdId": "b1",
      "tag": "",
      "px": "999",
      "sz": "3",
      "ordType": "limit",
      "side": "buy",
      "fillSz": "0",
      "fillPx": "",
      "tradeId": "",
      "accFillSz": "0",
      "pendingFillSz": "2",
      "pendingSettleSz": "1",
      "canceledSz": "1",
      "state": "live",
      "avgPx": "0",
      "cancelSource": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085"
    }
  ]
}
```

#### Response Example
| Parameter          | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sprdId             | String | spread ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ordId              | String | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| clOrdId            | String | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| tag                | String | Order tag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| px                 | String | Price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| sz                 | String | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ordType            | String | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`ioc`: Immediate-or-cancel order                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| side               | String | Order side                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| fillSz          | String | Last fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| fillPx      | String | Last fill price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| tradeId    | String | Last trade ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| accFillSz          | String | Accumulated fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| pendingFillSz      | String | Live quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| pendingSettleSz    | String | Quantity that's pending settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| canceledSz         | String | Quantity canceled due order cancellations or trade rejections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| avgPx              | String | Average filled price. If none is filled, it will return "0".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| state              | String | State <br>`canceled` <br>`live` <br>`partially_filled` <br>`filled`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| cancelSource       | String | Source of the order cancellation.Valid values and the corresponding meanings are: <br>`0`: Order canceled by system <br>`1`: Order canceled by user <br>`14`: Order canceled: IOC order was partially canceled due to incompletely filled<br>`15`: Order canceled: The order price is beyond the limit<br>`20`: Cancel all after triggered <br>`31`: The post-only order will take liquidity in maker orders<br>`32`: Self trade prevention<br>`34`: Order failed to settle due to insufficient margin <br>`35`: Order cancellation due to insufficient margin from another order<br>`44`: Your order was canceled because your available balance of this crypto was insufficient for auto conversion. Auto conversion was triggered when the total collateralized liabilities for this crypto reached the platform’s risk control limit. |
| uTime              | String | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| cTime              | String | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

<aside class="notice">
Order sizes equation: pendingFillSz + canceledSz + accFillSz = sz
</aside>
