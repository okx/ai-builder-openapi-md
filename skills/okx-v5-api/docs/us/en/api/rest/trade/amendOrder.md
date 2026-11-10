---
site: us
lang: en
name: POST / Amend order
method: POST
path: /api/v5/trade/amend-order
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### POST / Amend order

Amend an incomplete order.

#### Rate Limit: 60 requests per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 4 requests per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

Rate limit of this endpoint will also be affected by the rules [Sub-account rate limit] (/docs-v5/en/#overview-rate-limits-sub-account-rate-limit) and [Fill ratio based sub-account rate limit] (/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit).

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

| Parameter              | Type   | Required | Description                                                                                                                                                                                                                                                                                          |
|:-----------------------| :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                 | String | Yes       | Instrument ID                                                                                                                                                                                                                                                                                        |
| cxlOnFail              | Boolean | No       | Whether the order needs to be automatically canceled when the order amendment fails  <br>Valid options: `false` or `true`, the default is `false`. <br>Amendment failure scenarios include: `newSz` not a multiple of `lotSz`, position or risk limit breach, etc. When `false` (default): the original order continues unchanged after a failed amendment. When `true`: the original order is auto-cancelled on any amendment failure.                                                                                                                                                   |
| ordId                  | String | Conditional     | Order ID  <br>Either `ordId` or `clOrdId` is required. If both are passed, `ordId` will be used.                                                                                                                                                                                                     |
| clOrdId                | String | Conditional     | Client Order ID as assigned by the client                                                                                                                                                                                                                                                            |
| reqId                  | String | No       | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. <br>The response will include the corresponding `reqId` to help you identify the request if you provide it in the request.    |
| newSz                  | String | Conditional     | New **total** target quantity after amendment, must be > 0. This is the desired total order size, not the remaining unfilled portion. For a partially-filled order: if 3 contracts are already filled and you want a total of 8, pass `newSz=8` (not 5). The system will attempt to fill the remaining 5. At least one of `newSz` or `newPx` (or `newPxUsd`/`newPxVol` for options) must be provided.                                                                                                                                     |
| newPx                  | String | Conditional     | New price after amendment. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol. It must be consistent with parameters when placing orders. For example, if users placed the order using px, they should use newPx when modifying the order. At least one of `newSz` or `newPx` must be provided. |
| speedBump | String | Conditional | Speed bump<br>`1`: Event contract speed bumps (the delay duration will be changed subject to adjustment without prior notice). Required for non-post-only orders of `EVENTS` symbols. |
| newPxUsd               | String | Conditional | Modify options orders using USD prices <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                                                                 |
| newPxVol               | String | Conditional | Modify options orders based on implied volatility, where 1 represents 100% <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                             |
| pxAmendType            | String  | No          | The price amendment type for orders<br>`0`: Do not allow the system to amend to order price if `newPx` exceeds the price limit <br>`1`: Allow the system to amend the price to the best available value within the price limit if `newPx` exceeds the price limit<br> The default value is `0`         |
| attachAlgoOrds         | Array of objects   | No          | Attached TP/SL or trailing stop order information                                                                                                                                                                                                                                                        |
| > attachAlgoId         | String   | Conditional          | The order ID of the attached TP/SL or trailing stop order. It is required to identify the attached order when amending. It will not be posted to algoId when placing the attached algo order after the general order is filled completely.                                                                                               |
| > attachAlgoClOrdId    | String   | Conditional          | Client-supplied Algo ID when placing order with attached TP/SL or trailing stop<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.<br>It will be posted to `algoClOrdId` when placing the attached algo order once the general order is filled completely.                         |
| > newTpTriggerPx       | String | Conditional        | Take-profit trigger price. <br>Either the take profit trigger price or order price is 0, it means that the take profit is deleted.                                                                                                                                                                   |
| > newTpTriggerRatio    | String | Conditional        | Take profit trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP. <br> Only one of `newTpTriggerPx` and `newTpTriggerRatio` can be passed. <br>If the main order is a buy order, it must be greater than 0, and if the main order is a sell order, it must be between -1 and 0. 0 means to delete the take-profit.                                                                                                                                                                                                            |
| > newTpOrdPx           | String | Conditional        | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price.                                                                                                                                                                                                     |
| > newTpOrdKind         | String | No       | TP order kind<br>`condition`<br>`limit`                                                                                                                                                                                                                                                            |
| > newSlTriggerPx       | String | Conditional        | Stop-loss trigger price<br>Either the stop loss trigger price or order price is 0, it means that the stop loss is deleted.                                                                                                                                                                           |
| > newSlTriggerRatio    | String | Conditional        | Stop-loss trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP.<br> Only one of `newSlTriggerPx` and `newSlTriggerRatio` can be passed. <br>If the main order is a buy order, it should be between 0 and 1, and if the main order is a sell order, it must be greater than 0. <br> Only one of `newSlTriggerPx` and `newSlTriggerRatio` can be passed, 0 means to delete the stop-loss.                                                                                                                                                                                                        |
| > newSlOrdPx           | String | Conditional        | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price.                                                                                                                                                                                                         |
| > newTpTriggerPxType   | String | Conditional        | Take-profit trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price<br>Only applicable to `FUTURES`/`SWAP`<br>If you want to add the take-profit, this parameter is required                                                                                        |
| > newSlTriggerPxType   | String | Conditional        | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price<br>Only applicable to `FUTURES`/`SWAP`<br>If you want to add the stop-loss, this parameter is required                                                                                            |
| > sz                   | String | Conditional | New size. Only applicable to TP order of split TPs, and it is required for TP order of split TPs                                                                                                                                                                                                     |
| > amendPxOnTriggerType | String            | No          | Whether to enable Cost-price SL. Only applicable to SL order of split TPs. <br>`0`: disable, the default value <br>`1`: Enable                                                                                                                                                                       |
| > newCallbackRatio    | String | Conditional | New callback ratio, e.g. `0.05` represents 5%.<br>Either `newCallbackRatio` or `newCallbackSpread` can be passed. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > newCallbackSpread   | String | Conditional | New callback spread (price distance).<br>Either `newCallbackRatio` or `newCallbackSpread` can be passed. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > newActivePx         | String | No          | New activation price.<br>Only applicable when `ordType` = `move_order_stop` |
| rpiTakerAccess | Boolean | No | Default `false`.<br>When `true`, the amended order can access RPI liquidity, for `limit`, `market`, `fok`, and `ioc` orders.<br>Not inherited from the original order — must be re-specified on each amend request (omitted = `false` for that amend). |
| rpiPxRound | Boolean | No | Default `false`. Effective only on RPI maker orders (`ordType: rpi`). When `true`, a price that violates the RPI maker spacing rule is auto-rounded outward to the nearest placeable, non-crossing level instead of being rejected. Silently ignored on non-RPI orders and on `OPTION`/`EVENTS`. |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
         "clOrdId":"",
         "ordId":"12344",
         "ts":"1695190491421",
         "reqId":"b12344",
         "sCode":"0",
         "sMsg":""
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
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
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
