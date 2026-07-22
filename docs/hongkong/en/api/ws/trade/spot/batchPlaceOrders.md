---
site: hongkong
lang: en
name: WS / Place multiple orders
type: guide
---

### WS / Place multiple orders

Place orders in a batch. Maximum 20 orders can be placed per request<br><br>

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 300 orders per 2 seconds
#### Rate limit rule: User ID + Instrument ID

<aside class="notice">
Unlike other endpoints, the rate limit of this endpoint is determined by the number of orders. If there is only one order in the request, it will consume the rate limit of `Place order`.
</aside>

<aside class="notice">
Rate limit is shared with the `Place multiple orders` REST API endpoints
</aside>

> Request Example

```shell
{
  "id": "1513",
  "op": "batch-orders",
  "args": [
    {
      "side": "buy",
      "instId": "BTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "100"
    },
    {
      "side": "buy",
      "instId": "LTC-USDT",
      "tdMode": "cash",
      "ordType": "market",
      "sz": "1"
    }
  ]
}
```

#### Request Parameters

| Parameter      | Type    | Required    | Description                                                                                                                                                                                                                                                                                           |
|:---------------| :------ |:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String  | Yes         | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                   |
| op             | String  | Yes         | Operation<br>`batch-orders`                                                                                                                                                                                                                                                                           |
| args           | Array of objects   | Yes         | Request Parameters                                                                                                                                                                                                                                                                                    |
| > instId       | String  | Yes         | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                        |
| > tdMode       | String  | Yes         | Trade mode<br>`cash`                                                                                                                                                                                                                                                                                  |
| > clOrdId      | String  | No          | Client Order ID as assigned by the client  <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                     |
| > tag          | String  | No          | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                                                       |
| > side         | String  | Yes         | Order side<br>`buy`<br>`sell`                                                                                                                                                                                                                                                                         |
| > ordType      | String  | Yes         | Order type<br>`market`: market order <br>`limit`: limit order <br>`post_only`: Post-only order <br>`fok`: Fill-or-kill order <br>`ioc`: Immediate-or-cancel order <br>`elp`: Enhanced Liquidity Program order                                                                                                                                     |
| > sz           | String  | Yes         | Quantity to buy or sell.                                                                                                                                                                                                                                                                              |
| > px           | String  | Conditional | Order price. Only applicable to `limit`,`post_only`,`fok`,`ioc`,`mmp`,`mmp_and_post_only` order.                                                                                                                                                                                                      |
| > tgtCcy       | String  | No          | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency  <br>Only applicable to `SPOT` Market Orders<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                    |
| > banAmend     | Boolean | No          | Whether to disallow the system from amending the size of the SPOT Market Order.<br>Valid options: `true` or `false`. The default value is `false`.<br>If `true`, system will not amend and reject the market order if user does not have sufficient funds. <br>Only applicable to SPOT Market Orders |
| > stpId        | String | No          | ~~Self trade prevention ID. Orders from the same master account with the same ID will be prevented from self trade.<br>Numerical integers defined by user in the range of 1<= x<= 999999999~~  (Deprecated)                                                                                                            |
| > stpMode      | String | No          | Self trade prevention mode. <br>Default to cancel maker <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both does not support FOK.                                                                                                          |
| > isElpTakerAccess | Boolean | No | ELP taker access<br>`true`: the request can trade with ELP orders but a speed bump will be applied<br>`false`: the request cannot trade with ELP orders and no speed bump<br><br>The default value is `false` while `true` is only applicable to ioc orders. |
| expTime        | String | No          | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                               |
| tradeQuoteCcy   | String | No       | The quote currency used for trading. Only applicable to `SPOT`. <br> The default value is the quote currency of the `instId`, for example: for `BTC-USD`, the default is `USD`. |

> Response Example When All Succeed

```json
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": "",
      "subCode: ""
    },
    {
      "clOrdId": "",
      "ordId": "12344",
      "tag": "",
      "sCode": "0",
      "sMsg": "",
      "subCode: ""
    }
  ],
  "code": "0",
  "msg": "",
  "inTime": "1695190491421339",
  "outTime": "1695190491423240"
}
```

> Response Example When Partially Successful

```json
{
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "",
      "ordId": "12345689",
      "tag": "",
      "sCode": "0",
      "sMsg": "",
      "subCode": ""
    },
    {
      "clOrdId": "",
      "ordId": "",
      "tag": "",
      "sCode": "51008",
      "sMsg": "Order failed. Insufficient USDT balance in account",
      "subCode": "1000"
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
  "id": "1513",
  "op": "batch-orders",
  "data": [
    {
      "clOrdId": "oktswap6",
      "ordId": "",
      "tag": "",
      "sCode": "51008",
      "sMsg": "Order failed. Insufficient USDT balance in account",
      "subCode": "1000"
    },
    {
      "clOrdId": "oktswap7",
      "ordId": "",
      "tag": "",
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
  "id": "1513",
  "op": "batch-orders",
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
| > clOrdId | String | Client Order ID as assigned by the client        |
| > tag     | String | Order tag                          |
| > sCode   | String | Order status code, `0` means success |
| > sMsg    | String | Rejection or success message of event execution.               |
| > subCode    | String   | Sub-code of sCode.<br> Returns `""` when sCode is 0 (request successful).<br> When sCode is not 0 (request failed), returns the sub-code if available; otherwise returns `""`.|
| inTime | String | Timestamp at Websocket gateway when the request is received, Unix timestamp format in microseconds, e.g. `1597026383085123`|
| outTime | String | Timestamp at Websocket gateway when the response is sent, Unix timestamp format in microseconds, e.g. `1597026383085123`|

<aside class="notice">
clOrdId<br>
clOrdId is a user-defined unique ID used to identify the order. It will be included in the response parameters if you have specified during order submission, and can be used as a request parameter to the endpoints to query, cancel and amend orders. <br>
clOrdId must be unique among all pending orders and the current request.
</aside>

<aside class="notice">
Rate limit of orders tagged as isElpTakerAccess:true<br>
- 50 orders per 2 seconds per User ID per instrument ID.<br>
- This rate limit is shared in Place order/Place multiple orders endpoints in REST/WebSocket<br>
</aside>
