---
site: us
lang: en
name: WS / Cancel order
type: guide
---

### WS / Cancel order

Cancel an incomplete order

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

<aside class="notice">
Rate limit is shared with the `Cancel order` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1514",
  "op": "cancel-order",
  "args": [
    {
      "instIdCode": 123456,
      "ordId": "2510789768709120"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                           |
| :-------- | :----- | -------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id        | String | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`cancel-order`                                                                                                                                                                                                                        |
| args      | Array of objects  | Yes      | Request Parameters                                                                                                                                                                                                                                    |
| > instIdCode   | Integer   | Yes       | Instrument ID code                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > ordId   | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, ordId will be used                                                                                                                                                         |
| > clOrdId | String | Conditional | Client Order ID as assigned by the client  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                        |

> Successful Response Example

```json
{
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "ts": "1695190491421",
      "sCode": "0",
      "sMsg": ""
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
  "id": "1514",
  "op": "cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "ts": "1695190491421",
      "sCode": "5XXXX",
      "sMsg": "Order not exist"
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
  "id": "1514",
  "op": "cancel-order",
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
| > clOrdId | String | Client Order ID as assigned by the client    |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Order status message               |
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
Cancel order returns with sCode equal to 0. It is not strictly considered that the order has been canceled. It only means that your cancellation request has been accepted by the system server. The result of the cancellation is subject to the state pushed by the order channel or the get order state.<br>
</aside>
