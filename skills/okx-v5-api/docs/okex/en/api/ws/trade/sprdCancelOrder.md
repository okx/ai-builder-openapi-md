---
site: okex
lang: en
name: WS / Cancel order
type: guide
---

### WS / Cancel order

Cancel an incomplete order

#### URL Path

/ws/v5/business (required login)

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

<aside class="notice">
Rate limit is shared with the Nitro Spread `Cancel order` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1514",
  "op": "sprd-cancel-order",
  "args": [
    {
      "ordId": "2510789768709120"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | String | Yes | Unique identifier of the message provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`sprd-cancel-order` |
| args | Array of objects | Yes | Request Parameters |
| > ordId | String | Conditional | Order ID <br>Either ordId or clOrdId is required, if both are passed, ordId will be used |
| > clOrdId | String | Conditional | Client Order ID as assigned by the client <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |

> Successful Response Example

```json
{
  "id": "1514",
  "op": "sprd-cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": ""
}
```

> Failure Response Example

```json
{
  "id": "1514",
  "op": "sprd-cancel-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "sCode": "5XXXX",
      "sMsg": "Order not exist"
    }
  ],
  "code": "1",
  "msg": ""
}
```

> Response Example When Format Error

```json
{
  "id": "1514",
  "op": "sprd-cancel-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args"
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
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Order status message               |

<aside class="notice">
Cancel order returns with sCode equal to 0. It is not strictly considered that the order has been canceled. It only means that your cancellation request has been accepted by the system server. The result of the cancellation is subject to the state pushed by the sprd-orders channel or the get order state.<br>
</aside>
