---
site: turkey
lang: en
name: WS / Amend multiple orders
type: guide
---

### WS / Amend multiple orders

Amend incomplete orders in batches. Maximum 20 orders can be amended per request.

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 300 orders per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 4 orders per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

Rate limit of this endpoint will also be affected by the rules [Sub-account rate limit] (/docs-v5/en/#overview-rate-limits-sub-account-rate-limit) and [Fill ratio based sub-account rate limit] (/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit).

<aside class="notice">
Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Amend order`.
</aside>

<aside class="notice">
Rate limit is shared with the `Amend multiple orders` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1513",
  "op": "batch-amend-orders",
  "args": [
    {
      "instIdCode": 123456,
      "ordId": "12345689",
      "newSz": "2"
    },
    {
      "instIdCode": 123456,
      "ordId": "12344",
      "newSz": "2"
    }
  ]
}
```

#### Request Parameters

| Parameter   | Type    | Required | Description                                                                                                                                                                                                                                                                                          |
| :---------- | :------ | -------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id          | String  | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                  |
| op          | String  | Yes      | Operation<br>`batch-amend-orders`                                                                                                                                                                                                                                                                    |
| args        | Array of objects   | Yes      | Request Parameters                                                                                                                                                                                                                                                                                   |
| > instIdCode   | Integer   | Yes       | Instrument ID code                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > cxlOnFail | Boolean | No       | Whether the order needs to be automatically canceled when the order amendment fails  <br>Valid options: `false` or `true`, the default is `false`.                                                                                                                                                   |
| > ordId     | String  | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used.                                                                                                                                                                                                      |
| > clOrdId   | String  | Conditional | Client Order ID as assigned by the client                                                                                                                                                                                                                                                            |
| > reqId     | String  | No       | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                               |
| > newSz     | String  | Conditional | New quantity after amendment and it has to be larger than 0. Either `newSz` or `newPx` is required. When amending a partially-filled order, the `newSz` should include the amount that has been filled.                                                                                                   |
| > newPx     | String | Conditional     | New price after amendment. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol. It must be consistent with parameters when placing orders. For example, if users placed the order using px, they should use newPx when modifying the order. |
| > speedBump | String | Conditional | Speed bump<br>`1`: Event contract speed bumps (the delay duration will be changed subject to adjustment without prior notice). Required for non-post-only orders of `EVENTS` symbols. |
| > newPxUsd  | String | Conditional | Modify options orders using USD prices <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                                                                 |
| > newPxVol  | String | Conditional | Modify options orders based on implied volatility, where 1 represents 100% <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                             |
| > pxAmendType  | String  | No       | The price amendment type for orders<br>`0`: Do not allow the system to amend to order price if `newPx` exceeds the price limit <br>`1`: Allow the system to amend the price to the best available value within the price limit if `newPx` exceeds the price limit<br> The default value is `0`         |
| > rpiTakerAccess | Boolean | No | Default `false`.<br>When `true`, the amended order can access RPI liquidity, for `limit`, `market`, `fok`, and `ioc` orders.<br>Not inherited from the original order — must be re-specified on each amend request (omitted = `false` for that amend). |
| > rpiPxRound | Boolean | No | Default `false`. Effective only on RPI maker orders (`ordType: rpi`). When `true`, a price that violates the RPI maker spacing rule is auto-rounded outward to the nearest placeable, non-crossing level instead of being rejected. Silently ignored on non-RPI orders and on `OPTION`/`EVENTS`. |
| expTime     | String | No     | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                              |

> Response Example When All Succeed

```json
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "12345689",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": "",
      "subCode": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "12344",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": "",
      "subCode": ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When All Failed

```json
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "ts": "1695190491421",
      "reqId": "b12344",
		  "sCode": "51008",
      "sMsg": "Order failed. Insufficient USDT balance in account",
      "subCode": "1000"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When Partially Successful

```json
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "",
      "ordId": "oktswap7",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "51063",
      "sMsg": "OrdId does not exist"
      "subCode": ""
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When Format Error

```json
{
  "id": "1513",
  "op": "batch-amend-orders",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

#### Response Parameters

| Parameter | Type   | Description                                                                                                         |
| :-------- | :----- | :------------------------------------------------------------------------------------------------------------------ |
| id        | String | Unique identifier of the message                                                                                    |
| op        | String | Operation                                                                                                           |
| code      | String | Error Code                                                                                                          |
| msg       | String | Error message                                                                                                       |
| data      | Array of objects  | Data                                                                                                                |
| > ordId   | String | Order ID                                                                                                            |
| > clOrdId | String | Client Order ID as assigned by the client                                                                           |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > reqId   | String | Client Request ID as assigned by the client for order amendment <br>If the user provides reqId in the request, the corresponding reqId will be returned |
| > sCode   | String | Order status code, `0` means success                                                                                  |
| > sMsg    | String | Order status message                                                                                                |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
newSz <br>
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.
</aside>
