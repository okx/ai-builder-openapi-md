---
site: okex
lang: en
name: Cancel order
method: POST
path: /api/v5/sprd/cancel-order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel order

Cancel an incomplete order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/sprd/cancel-order`

> Request Example

```shell
POST /api/v5/sprd/cancel-order
body
{
    "ordId":"2510789768709120"
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

# cancel order
result = spreadAPI.cancel_order(ordId='1905309079888199680')
print(result)
```

#### Request Parameters
| Parameter | Type   | Required    | Description                                                                               |
|-----------|--------|-------------|-------------------------------------------------------------------------------------------|
| ordId     | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required. If both are passed, `ordId` will be used. |
| clOrdId   | String | Conditional | Client Order ID as assigned by the client                                                 |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### Response Example
| Parameter | Type   | Description                                              |
|-----------|--------|----------------------------------------------------------|
| ordId     | String | Order ID                                                 |
| clOrdId   | String | Client Order ID as assigned by the client                |
| sCode     | String | The code of the event execution result, 0 means success. |
| sMsg      | String | Rejection message if the request is unsuccessful.        |

<aside class="notice">
Cancel order returns with sCode equal to 0. It is not strictly considered that the order has been canceled. It only means that your cancellation request has been accepted by the system server. The result of the cancellation is subject to the state pushed by the order channel or the get order state.
</aside>
