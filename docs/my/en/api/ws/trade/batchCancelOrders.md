---
site: my
lang: en
name: WS / Cancel multiple orders
type: guide
---

### WS / Cancel multiple orders

Cancel incomplete orders in batches. Maximum 20 orders can be canceled per request.

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 300 orders per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

<aside class="notice">
Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Cancel order`.
</aside>

<aside class="notice">
Rate limit is shared with the `Cancel multiple orders` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "args": [
    {
      "instIdCode": 123456,
      "ordId": "2517748157541376"
    },
    {
      "instIdCode": 654321,
      "ordId": "2517748155771904"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                           |
| :-------- | :----- | -------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id        | String | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`batch-cancel-orders`                                                                                                                                                                                                                 |
| args      | Array of objects  | Yes      | Request Parameters                                                                                                                                                                                                                                    |
| > instIdCode   | Integer   | Yes       | Instrument ID code                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > ordId   | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, ordId will be used                                                                                                                                                         |
| > clOrdId | String | Conditional | Client Order ID as assigned by the client  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                        |

> Response Example When All Succeed

```json
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "ts": "1695190491421",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
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

> Response Example When partially successfully

```json
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "ts": "1695190491421",
      "sCode": "0",
      "sMsg": ""
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "ts": "1695190491421",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "2",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When All Failed

```json
{
  "id": "1515",
  "op": "batch-cancel-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "2517748157541376",
      "ts": "1695190491421",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "2517748155771904",
      "ts": "1695190491421",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
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
  "id": "1515",
  "op": "batch-cancel-orders",
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
| > clOrdId | String | Client Order ID as assigned by the client           |
| > ts    | String  | Order creation time. Unix timestamp format in milliseconds, e.g. `1597026383085`. Equivalent to `cTime` in the order channel. |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Order status message               |
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|
