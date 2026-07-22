---
site: okex
lang: en
name: Get active orders
method: GET
path: /api/v5/sprd/orders-pending
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get active orders

Retrieve all incomplete orders under the current account.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/sprd/orders-pending`

> Request Example

```shell
GET /api/v5/sprd/orders-pending
```
```python
import okx.SpreadTrading as SpreadTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# get active orders
result = spreadAPI.get_active_orders()
print(result)
```

#### Request Parameters
| Parameter | Type   | Required | Description                                                                                                                             |
|-----------|--------|----------|-----------------------------------------------------------------------------------------------------------------------------------------|
| sprdId    | String | No       | spread ID, e.g.                                                                                                                         |
| ordType   | String | No       | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`ioc`: Immediate-or-cancel order                                                                                                       |
| state     | String | No       | State <br>`live` <br>`partially_filled`                                                                                                     |
| beginId   | String | No       | Start order ID the request to begin with. Pagination of data to return records newer than the requested order Id, not including beginId |
| endId     | String | No       | End order ID the request to end with. Pagination of data to return records earlier than the requested order Id, not including endId     |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100                                                                   |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "sprdId": "BTC-USDT_BTC-UST-SWAP",
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
| Parameter          | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|--------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sprdId             | String | spread ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ordId              | String | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| clOrdId            | String | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| tag                | String | Order tag                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| px                 | String | Price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| sz                 | String | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ordType            | String | Order type<br>`market`: Market order <br>`limit`: Limit order <br>`post_only`: Post-only order <br>`ioc`: Immediate-or-cancel order                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| side               | String | Order side                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| fillSz          | String | Last fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| fillPx      | String | Last fill price                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| tradeId    | String | Last trade ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| accFillSz          | String | Accumulated fill quantity                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| pendingFillSz      | String | Quantity still remaining to be filled                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| pendingSettleSz    | String | Quantity that's pending settlement                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| canceledSz         | String | Quantity canceled due order cancellations or trade rejections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| avgPx              | String | Average filled price. If none is filled, it will return "0".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| state              | String | State <br>`live` <br>`partially_filled`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| cancelSource       | String | Source of the order cancellation.Valid values and the corresponding meanings are: <br>`0`: Order canceled by system <br>`1`: Order canceled by user <br>`14`: Order canceled: IOC order was partially canceled due to incompletely filled<br>`15`: Order canceled: The order price is beyond the limit<br> `20`: Cancel all after triggered <br>`31`: The post-only order will take liquidity in maker orders<br> `32`: Self trade prevention <br>`34`: Order failed to settle due to insufficient margin <br>`35`: Order cancellation due to insufficient margin from another order<br>`44`: Your order was canceled because your available balance of this crypto was insufficient for auto conversion. Auto conversion was triggered when the total collateralized liabilities for this crypto reached the platform’s risk control limit. |
| uTime              | String | Update time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| cTime              | String | Creation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
