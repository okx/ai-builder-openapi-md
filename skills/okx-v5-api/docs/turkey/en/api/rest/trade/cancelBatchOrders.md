---
site: turkey
lang: en
name: POST / Cancel multiple orders
method: POST
path: /api/v5/trade/cancel-batch-orders
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Cancel multiple orders

Cancel incomplete orders in batches. Maximum 20 orders can be canceled per request. Request parameters should be passed in the form of an array.

#### Rate Limit: 300 orders per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

<aside class="notice">
Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Cancel order`.
</aside>

#### HTTP Request

`POST /api/v5/trade/cancel-batch-orders`

> Request Example

```shell
POST /api/v5/trade/cancel-batch-orders
body
[
    {
        "instId":"BTC-USDT",
        "ordId":"590908157585625111"
    },
    {
        "instId":"BTC-USDT",
        "ordId":"590908544950571222"
    }
]
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Cancel multiple orders by ordId
cancel_orders_with_orderId = [
    {"instId": "BTC-USDT", "ordId": "590908157585625111"},
    {"instId": "BTC-USDT", "ordId": "590908544950571222"}
]

result = tradeAPI.cancel_multiple_orders(cancel_orders_with_orderId)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                    |
| :------ | :----- | :------- |:-----------------------------------------------------------------------------------------------|
| instId  | String | Yes          | Instrument ID, e.g. `BTC-USDT`                                                                 |
| ordId   | String | Conditional     | Order ID<br>Either `ordId` or `clOrdId` is required. If both are passed, `ordId` will be used. |
| clOrdId | String | Conditional     | Client Order ID as assigned by the client                                                      |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                      |
| :--------- | :------- | :---------------------------- |
| code            | String   | The result code, `0` means success |
| msg             | String   | The error message, empty if the code is 0 |
| data            | Array of objects | Array of objects contains the response results |
| > ordId      | String   | Order ID                        |
| > clOrdId    | String   | Client Order ID as assigned by the client              |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > sCode      | String   | The code of the event execution result, `0` means success. |
| > sMsg       | String   | Rejection message if the request is unsuccessful.         |
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123` <br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|
