---
site: my
lang: en
name: WS / Amend order
type: guide
---

### WS / Amend order

Amend an incomplete order.

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 60 requests per 2 seconds
#### Rate Limit of lead trader lead instruments for Copy Trading: 4 requests per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

Rate limit of this endpoint will also be affected by the rules [Sub-account rate limit] (/docs-v5/en/#overview-rate-limits-sub-account-rate-limit) and [Fill ratio based sub-account rate limit] (/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit).

<aside class="notice">
Rate limit is shared with the `Amend order` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1512",
  "op": "amend-order",
  "args": [
    {
      "instIdCode": 123456,
      "ordId": "2510789768709120",
      "newSz": "2"
    }
  ]
}
```

#### Request Parameters

| Parameter   | Type    | Required | Description                                                                                                                                                                                                                                                                                          |
|:------------| :------ | -------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id          | String  | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                  |
| op          | String  | Yes      | Operation<br>`amend-order`                                                                                                                                                                                                                                                                           |
| args        | Array of objects   | Yes      | Request Parameters                                                                                                                                                                                                                                                                                   |
| > instIdCode   | Integer   | Yes       | Instrument ID code                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > cxlOnFail | Boolean | No       | Whether the order needs to be automatically canceled when the order amendment fails  <br>Valid options: `false` or `true`, the default is `false`.                                                                                                                                                   |
| > ordId     | String  | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used.                                                                                                                                                                                                      |
| > clOrdId   | String  | Conditional | Client Order ID as assigned by the client                                                                                                                                                                                                                                                            |
| > reqId     | String  | No       | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                               |
| > newSz     | String  | Conditional | New quantity after amendment and it has to be larger than 0. Either `newSz` or `newPx` is required. When amending a partially-filled order, the `newSz` should include the amount that has been filled.                                                                                              |
| > newPx     | String | Conditional     | New price after amendment. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol. It must be consistent with parameters when placing orders. For example, if users placed the order using px, they should use newPx when modifying the order. |
| > speedBump | String | Conditional | Speed bump<br>`1`: Event contract speed bumps (the delay duration will be changed subject to adjustment without prior notice). Required for non-post-only orders of `EVENTS` symbols. |
| > newPxUsd  | String | Conditional | Modify options orders using USD prices <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                                                                 |
| > newPxVol  | String | Conditional | Modify options orders based on implied volatility, where 1 represents 100% <br>Only applicable to options. <br>When modifying options orders, users can only fill in one of the following: newPx, newPxUsd, or newPxVol.                                                                             |
| > pxAmendType  | String  | No       | The price amendment type for orders<br>`0`: Do not allow the system to amend to order price if `newPx` exceeds the price limit <br>`1`: Allow the system to amend the price to the best available value within the price limit if `newPx` exceeds the price limit<br> The default value is `0`         |
| expTime     | String | No     | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                              |

> Successful Response Example

```json
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
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

> Failure Response Example

```json
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "ts": "1695190491421",
      "reqId": "b12344",
      "sCode": "51008",
      "sMsg": "Order failed. Insufficient USDT balance in account",
      "subCode": "10000"
    }
  ],
  "code": "1",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When Format Error

```json
{
  "id": "1512",
  "op": "amend-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

#### Response Parameters

| Parameter | Type   | Description                        |
| :-------- | :----- | :--------------------------------- |
| id        | String | Unique identifier of the message   |
| op        | String | Operation                          |
| code      | String | Error Code                         |
| msg       | String | Error message                      |
| data      | Array of objects  | Data                               |
| > ordId   | String | Order ID                           |
| > clOrdId | String | Client Order ID as assigned by the client   |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > reqId   | String | Client Request ID as assigned by the client for order amendment         |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Order status message               |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
newSz <br>
If the new quantity of the order is less than or equal to the filled quantity when you are amending a partially-filled order, the order status will be changed to filled.
</aside>

<aside class="notice">
The amend order returns sCode equal to 0. It is not strictly considered that the order has been amended. It only means that your amend order request has been accepted by the system server. The result of the amend is subject to the status pushed by the order channel or the order status query
</aside>
