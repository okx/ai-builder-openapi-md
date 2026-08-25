---
site: okex
lang: en
name: Websocket Trade API
type: guide
---

## Websocket Trade API
### WS / Place order

You can place an order only if you have sufficient funds.<br>

#### URL Path

/ws/v5/business (required login)

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

<aside class="notice">
Rate limit is shared with the Nitro Spread `Place order` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1512",
  "op": "sprd-order",
  "args": [
    {
       "sprdId":"BTC-USDT_BTC-USDT-SWAP",
       "clOrdId":"b15",
       "side":"buy",
       "ordType":"limit",
       "px":"2.15",
       "sz":"2"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | String | Yes | Unique identifier of the message provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`sprd-order` |
| args | Array of objects | Yes | Request parameters |
| > sprdId | String | Yes | spread ID, e.g. BTC-USDT_BTC-USD-SWAP |
| > clOrdId | String | No | Client Order ID as assigned by the client <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| > tag | String | No | Order tag <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |
| > side | String | Yes | Order side <br>`buy` <br>`sell` |
| > ordType | String | Yes | Order type:<br>`market`: Market order <br>`limit`: Limit order <br/ >`post_only`: Post-only order <br>`ioc`: Immediate-or-cancel order|
| > sz | String | Yes | Quantity to buy or sell |
| > px | String | Yes | Order price. Only applicable to `limit, post_only, ioc` order. |

> ##### Successful Response Example

```json
{
  "id": "1512",
  "op": "sprd-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
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
  "op": "sprd-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "5XXXX",
      "sMsg": "not exist"
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
  "op": "sprd-order",
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
| > clOrdId | String | Client Order ID as assigned by the client       |
| > tag     | String | Order tag                          |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Rejection or success message of event execution.               |

<aside class="notice">
clOrdId<br>
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders. <br>
clOrdId must be unique among the clOrdIds of all pending orders.
</aside>
