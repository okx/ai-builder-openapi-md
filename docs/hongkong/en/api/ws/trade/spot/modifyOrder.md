---
site: hongkong
lang: en
name: WS / Amend order
type: guide
---

### WS / Amend order

Amend an incomplete order.

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

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
      "instId": "BTC-USDT",
      "ordId": "2510789768709120",
      "newSz": "2"
    }
  ]
}
```

#### Request Parameters

| Parameter   | Type     | Required    | Description                                                                                                                                                                                                                                         |
|:------------|:---------|-------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id          | String   | Yes         | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op          | String   | Yes         | Operation<br>`amend-order`                                                                                                                                                                                                                          |
| args        | Array of objects    | Yes         | Request Parameters                                                                                                                                                                                                                                  |
| > instId    | String   | Yes         | Instrument ID                                                                                                                                                                                                                                       |
| > cxlOnFail | Boolean  | No          | Whether the order needs to be automatically canceled when the order amendment fails<br>Valid options: false or true, the default is false.                                                                                                          |
| > ordId     | String   | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used.                                                                                                                                                     |
| > clOrdId   | String   | Conditional | Client Order ID as assigned by the client                                                                                                                                                                                                           |
| > reqId     | String   | No          | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                              |
| > newSz     | String   | Conditional | New quantity after amendment. Either `newSz` or `newPx` is required. When amending a partially-filled order, the `newSz` should include the amount that has been filled.                                                                            |
| > newPx     | String   | Conditional | New price after amendment.                                                                                                                                                                                                                          |
| expTime     | String   | No          | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                             |

> Successful Response Example

```json
{
  "id": "1512",
  "op": "amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
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
