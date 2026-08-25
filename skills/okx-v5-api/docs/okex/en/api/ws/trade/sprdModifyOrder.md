---
site: okex
lang: en
name: WS / Amend order
type: guide
---

### WS / Amend order

Amend an incomplete order.

#### URL Path

/ws/v5/business (required login)

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: User ID

<aside class="notice">
Rate limit is shared with the `Amend order` REST API endpoints
</aside>

> Request Example

```shell
{
   "id":"1512",
   "op":"sprd-amend-order",
   "args":[
      {
         "ordId":"2510789768709120",
         "newSz":"2"
      }
   ]
}

```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | String | Yes | Unique identifier of the messageProvided by client. <br>It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`sprd-amend-order` |
| args | Array of objects | Yes | Request Parameters |
| > ordId | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required, if both are passed, `ordId` will be used. |
| > clOrdId | String | Conditional | Client Order ID as assigned by the client |
| > reqId | String | No | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| > newSz | String | Conditional | New quantity after amendment. <br>Either `newSz` or `newPx` is required. When amending a partially-filled order, the newSz should include the amount that has been filled and failed. |
| > newPx | String | Conditional | New price after amendment. |

> Successful Response Example

```json
{
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
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
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": ""
}

```

> Response Example When Format Error

```json
{
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args"
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| id | String | Unique identifier of the message |
| op | String | Operation |
| code | String | Error Code |
| msg | String | Error message |
| data | Array of objects | Data |
| > ordId | String | Order ID |
| > clOrdId | String | Client Order ID as assigned by the client |
| > reqId | String | Client Request ID as assigned by the client for order amendment |
| > sCode | String | Order status code, 0 means success |
| > sMsg | String | Order status message |

<aside class="notice">
newSz<br>
If the new quantity of the order is less than or equal to the (accFillSz + canceledSz + pendingSettleSz), after pendingSettleSz is settled, the order status will be transitioned into filled (if canceledSz = 0), or canceled (if canceledSz > 0).
</aside>

<aside class="notice">
The amend order returns sCode equal to 0<br>

It is not strictly considered that the order has been amended. It only means that your amend order request has been accepted by the system server. The result of the amend is subject to the status pushed by the order channel or the order status query.
</aside>
