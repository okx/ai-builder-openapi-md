---
site: okex
lang: en
name: Amend order
method: POST
path: /api/v5/sprd/amend-order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Amend order

Amend an incomplete order.

#### Rate Limit: 20 requests per 2 seconds

#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/sprd/amend-order`

> Request Example

```shell
POST /api/v5/sprd/amend-order
body
{
    "ordId":"2510789768709120",
    "newSz":"2"
}

```

#### Response parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| ordId | String | Conditional | Order ID <br>Either `ordId` or `clOrdId` is required. If both are passed, ordId will be used. |
| clOrdId | String | Conditional | Client Order ID as assigned by the client |
| reqId | String | No | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. <br>The response will include the corresponding reqId to help you identify the request if you provide it in the request. |
| newSz | String | Conditional | New quantity after amendment <br>Either `newSz` or `newPx` is required. <br>When amending a partially-filled order, the newSz should include the amount that has been filled. |
| newPx | String | Conditional | New price after amendment <br>Either `newSz` or `newPx` is required. |

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
         "sMsg":""
        }
    ]
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| ordId | String | Order ID |
| clOrdId | String | Client Order ID as assigned by the client. |
| reqId | String | Client Request ID as assigned by the client for order amendment. |
| sCode | String | The code of the event execution result, 0 means success. |
| sMsg | String | Rejection message if the request is unsuccessful. |

<aside class="notice">
newSz<br>
If the new quantity of the order is less than or equal to the (accFillSz + canceledSz + pendingSettleSz), after pendingSettleSz is settled, the order status will be transitioned into filled (if canceledSz = 0), or canceled (if canceledSz > 0).
</aside>

<aside class="notice">
The amend order returns sCode equal to 0<br>

It is not strictly considered that the order has been amended. It only means that your amend order request has been accepted by the system server. The result of the amend is subject to the status pushed by the order channel or the order status query.
</aside>
