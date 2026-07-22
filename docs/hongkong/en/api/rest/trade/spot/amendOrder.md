---
site: hongkong
lang: en
name: POST / Amend order
method: POST
path: /api/v5/trade/amend-order
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend order

Amend an incomplete order.

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/trade/amend-order`

> Request Example

```shell
POST /api/v5/trade/amend-order
body
{
    "ordId":"590909145319051111",
    "newSz":"2",
    "instId":"BTC-USDT"
}
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Amend order
result = tradeAPI.amend_order(
    instId="BTC-USDT",
    ordId="590909145319051111",
    newSz="2"
)
print(result)
```

#### Request Parameters

| Parameter              | Type            | Required    | Description                                                                                                                                                                                                                                                                                                                                                 |
|:-----------------------|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                 | String          | Yes         | Instrument ID                                                                                                                                                                                                                                                                                                                                               |
| cxlOnFail              | Boolean         | No          | Whether the order needs to be automatically canceled when the order amendment fails  <br>Valid options: `false` or `true`, the default is `false`.                                                                                                                                                                                                          |
| ordId                  | String          | Conditional | Order ID  <br>Either `ordId` or `clOrdId` is required. If both are passed, `ordId` will be used.                                                                                                                                                                                                                                                            |
| clOrdId                | String          | Conditional | Client Order ID as assigned by the client                                                                                                                                                                                                                                                                                                                   |
| reqId                  | String          | No          | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. <br>The response will include the corresponding `reqId` to help you identify the request if you provide it in the request.                                                           |
| newSz                  | String          | Conditional | New quantity after amendment. When amending a partially-filled order, the `newSz` should include the amount that has been filled.                                                                                                                                                                                                                           |
| newPx                  | String          | Conditional | New price after amendment.                                                                                                                                                                                                                                                                                                                                  |
| attachAlgoOrds         | Array of objects | No          | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                                                                               |
| > attachAlgoId         | String          | Conditional | The order ID of the attached TP/SL or trailing stop order. It is required to identify the attached order when amending. It will not be posted to algoId when placing the attached algo order after the general order is filled completely.                                                                                                                                                      |
| > attachAlgoClOrdId    | String          | Conditional | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                                                                                |
| > newTpTriggerPx       | String          | Conditional | Take-profit trigger price. <br>Either the take profit trigger price or order price is 0, it means that the take profit is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures.                                                                                                                                                              |
| > newTpOrdPx           | String          | Conditional | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures.                                                                                                                                                                                                |
| > newSlTriggerPx       | String          | Conditional | Stop-loss trigger price<br>Either the stop loss trigger price or order price is 0, it means that the stop loss is deleted.<br>Only applicable to Expiry Futures and Perpetual Futures.                                                                                                                                                                      |
| > newSlOrdPx           | String          | Conditional | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price.<br>Only applicable to Expiry Futures and Perpetual Futures.                                                                                                                                                                                                    |
| > newTpTriggerPxType   | String          | Conditional | Take-profit trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price<br>Only applicable to `FUTURES`/`SWAP`<br>If you want to add the take-profit, this parameter is required                                                                                                                                               |
| > newSlTriggerPxType   | String          | Conditional | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price<br>Only applicable to `FUTURES`/`SWAP`<br>If you want to add the stop-loss, this parameter is required                                                                                                                                                   |
| > sz                   | String          | Conditional | New size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs                                                                                                                                                                                                                                                            |
| > amendPxOnTriggerType | String          | No          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                                                                                                              |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
         "clOrdId":"",
         "ordId":"12344",
         "reqId":"b12344",
         "sCode":"0",
         "sMsg":"",
         "subCode": ""
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
| > clOrdId    | String   | Client Order ID as assigned by the client                  |
| > reqId      | String   | Client Request ID as assigned by the client for order amendment.      |
| > sCode      | String   | The code of the event execution result, `0` means success. |
| > sMsg       | String   | Rejection message if the request is unsuccessful.         |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at REST gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123` <br>The time is recorded after authentication. |
| outTime | String | Timestamp at REST gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
newSz <br>
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.
</aside>

<aside class="notice">
The amend order returns sCode equal to 0. It is not strictly considered that the order has been amended. It only means that your amend order request has been accepted by the system server. The result of the amend is subject to the status pushed by the order channel or the order status query
</aside>
