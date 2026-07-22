---
site: okex
lang: en
name: SBE Order book
method: GET
path: /api/v5/market/books-sbe
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

## SBE Market Data

### Overview

OKX supports Simple Binary Encoding (SBE) for data returned from the following WebSocket channels:

* [WS / Trades channel](/docs-v5/en/#order-book-trading-market-data-ws-trades-channel): `trades`
* [WS / Order book channel](/docs-v5/en/#order-book-trading-market-data-ws-order-book-channel): `bbo-tbt` and `books-l2-tbt`

### XML Schema

The SBE XML schema is now available for download:

<a href="/docs-v5/log_en/xml/okx_sbe_1_0.xml" download="okx_sbe_1_0.xml" class="btn btn-primary">
  <i class="fa fa-download"></i> Download XML Schema
</a>

### General Information

* The `bbo-tbt` channel is **available to users of any trading fee tier** but requires login. The `trades` and `books-l2-tbt` channels are restricted to users with a trading fee tier of **VIP4** or above in the live trading environment. Other users will receive error code 64003. In the demo trading environment, these channels require **VIP1** or above.<br>
* SBE channels will use a new WebSocket URL.<br>
  Live trading: `wss://ws.okx.com:8443/ws/v5/public-sbe`<br>
  Demo trading: `wss://wspap.okx.com:8443/ws/v5/public-sbe` <br>
* Both JSON and SBE format data will be available on the same connection, distinguishable by WebSocket frame type. opcode `1` indicates JSON, while opcode `2` indicates SBE.<br>
* Prices and quantities will be encoded as exponential decimals, using a signed integer mantissa and signed exponent. For example, a mantissa of 123456 and a exponent of -4 represents 12.3456 (actual value = mantissa * 10 ^ exponent).<br>
* The SBE protocol will use `instIdCode` , an integer will be provided by [Get instruments](/docs-v5/en/#public-data-rest-api-get-instruments) to represent trading instruments. Users must map `instIdCode` to `instId`, noting that `instIdCode` will change if a trading symbol is relisted, but `instIdCode` will remains unchanged when `instId` is renamed.<br>
* `tsUs` and `outTime` come from different servers, so their relative order is not guaranteed.
* `tsUs` is in microseconds format but only accurate to milliseconds. The microseconds-format timestamp is obtained by appending 000 to the millisecond timestamp.  For example, if the millisecond timestamp is 1726233600001, the related microseconds-format timestamp (tsUs) will be 1726233600001000.

### Integration Information

* To log in, transmit your API key and signature in the WebSocket connection header.<br>
  - The connection requests must contain the following headers:<br>
        - `OK-ACCESS-KEY` The API key as a String.<br>
        - `OK-ACCESS-SIGN` The Base64-encoded signature.<br>
        - `OK-ACCESS-TIMESTAMP`  Unix Epoch time in seconds. e.g : `1751335333`<br>
        - `OK-ACCESS-PASSPHRASE` The passphrase you specified when creating the API key.<br>
  - The `OK-ACCESS-SIGN` header is generated as follows:<br>
        - Create a pre-hash string of timestamp + method + requestPath<br>
        - Prepare the SecretKey.<br>
        - Sign the pre-hash string with the SecretKey using the HMAC SHA256.<br>
        - Encode the signature in the Base64 format. Example: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp + 'GET' + '/users/self/verify', SecretKey))<br>
        - Example of timestamp: const timestamp = '' + Date.now() / 1,000, e.g. 1704876947<br>
        - Method: always 'GET'.<br>
        - RequestPath : always '/users/self/verify'<br>
  - The response HTTP code of `101` indicates the successful login.<br>
  - The response HTTP code `401`, along with an error message in the response body, indicates a failed login. The error message will be in JSON fromat.<br>

```shell
Login error message example
{
    "msg": "Invalid apiKey",
    "code": "60005",
    "connId":"24a2aea3"
}
```

* Subscription request must be sent in JSON format. The response will also be in JSON format, and can be identified by opcode `1`.<br>
  - The protocol is similar to existing JSON-formatted subscription requests/response.<br>
  - The difference is that `instIdCode` should be used instead of instId.<br>

```shell
Subscription request example
{
    "op": "subscribe",
    "args": [
        {
            "channel": "trades",
            "instIdCode": 211874
        }
    ]
}

Subscription response example
{
    "event": "subscribe",
    "arg": {
        "channel": "trades",
        "instIdCode": 211874
    },
    "connId": "accb8e21"
}
```

* The notice event is supported in JSON format: 

```shell
Notice event example
{
    "event": "notice",
    "code": "64008",
    "msg": "The connection will soon be closed for a service upgrade. Please reconnect.",
    "connId": "a4d3ae55"
}
```

* The WebSocket server will send a ping frame with opcode `9` every 20 seconds after receiving a pong frame.<br>
  - If the WebSocket server does not receive a pong frame back from the connection within 60 secondes, the connection will be disconnected.<br>
  - Upon receiving a ping, you must respond with a pong frame using opcode `10`, along with a copy of the ping‘s payload as soon as possible (payload will be a random numerical text like 11446744073709551615).<br>
  - Unsolicited pong frames are permitted but will not prevent disconnection. It is advisable that the payload for these pong frames be empty.<br>

* For `trades` `bbo-tbt` and `books-l2-tbt` channels, data will be returned in binary format and can be identified by opcode `2`,  distinguishable by template ID. Key differences compared to existing JSON-formatted connections include:<br>
  - For the `trades` channel, the `seqId` will be returned.<br>
  - For the `bbo-tbt` channel, it usually provides real-time data, but under system overload, data loss can occur, varying by different connection.<br>
  - For the `books-l2-tbt`:<br>
        - When prices and quantities decimals change, a exponent update (template ID: 1002) will occur with previous sequence ID and sequence ID, identifiable by template ID. This must be processed to maintain the sequence ID consistence.<br>
        - The checksum will no longer be included.<br>
        - There will be no initial order book snapshot after subscription. Instead, OKX will provide a REST API endpoint that returns SBE binary data for the initial 400 levels snapshot. This endpoint will buffer requests and return data only when a new snapshot is generated, approximately every 500 ms.<br>

* The relationship between the channel and event is not one-to-one. The books-l2-tbt contains two types of events. The mapping is outlined below.

| Channel   | XML Template ID and message name |
| :----- | :------- |
| bbo-tbt | 1000: BboTbtChannelEvent      | 
| books-l2-tbt | 1001: BooksL2TbtChannelEvent<br>1002: BooksL2TbtExponentUpdateEvent | 
| books-l2-tbt-elp <br>(It is not enabled) | 1003: BooksL2TbtElpChannelEvent<br>1004: BooksL2TbtElpExponentUpdateEvent |
| trades | 1005: TradesChannelEvent |

* How to manage a local order book correctly
  1. Open a SBE WebSocket connection and subscribe to `books-l2-tbt`.
  2. Buffer the events received from the stream. Record the prevSeqId of the first event you received. <br>Note: For template ID 1002, the event is an exponent update, containing only exponent update information without ask or bid data. For template ID 1001, the data includes both asks and bids.
  3. Get a depth snapshot from `/books-sbe`, e.g. `https://openapi.okx.com/api/v5/market/books-sbe?instIdCode=12345&source=0`
  4. If the `seqId` from the snapshot is strictly less than the `prevSeqId` from step 2, go back to step 3.
  5. In the buffered events, discard any event where stream `seqId` is <= snapshot `seqId` of the snapshot. 
  6. The first buffered event should satisfy the condition: stream `prevSeqId` <= snapshot `seqId` < stream `seqId`.
  7. Set your local order book to the snapshot. Its sequence ID is snapshot `seqId`.
  8. Apply the update procedure below to all buffered events, and then to all subsequent events received.
      * If the template ID is 1002 (BooksL2TbtExponentUpdateEvent), only update the exponents without bid and ask data. If the template ID is 1001 (BooksL2TbtChannelEvent), follow the process outlined below.
      * For each price level in bids and asks, set the new quantity in the order book:
          - If the price level does not exist in the order book, insert it with new quantity.
          - If the quantity is zero, remove the price level from the order book.
      * Set the order book sequence ID to the latest sequence ID (`seqId`) in the processed event. <br>
Note: Not all snapshot `seqId` will appear in the `books-l2-tbt` channels.

* Sequence ID<br>

`seqId` is the sequence ID of the market data published. The set of sequence ID received by users is the same if users are connecting to the same channel through multiple websocket connections. Each `instIdCode` has an unique set of sequence ID. Users can use `prevSeqId` and `seqId` to build the message sequencing for incremental order book updates. Generally the value of seqId is larger than prevSeqId. The `prevSeqId` in the new message matches with `seqId` of the previous message. The smallest possible sequence ID value is 0, except in snapshot messages where the prevSeqId is always -1.<br>

Exceptions:<br>
1. If there are no updates to the depth for an extended period(Around 60 seconds), for the channel that always updates snapshot data, OKX will send the latest snapshot, for the channel that has incremental data, OKX will send a message with numInGroup: 0 to inform users that the connection is still active. `seqId` is the same as the last sent message and `prevSeqId` equals to `seqId`.<br>
2. The sequence number may be reset due to maintenance, and in this case, users will receive an incremental message with `seqId` smaller than `prevSeqId`. However, subsequent messages will follow the regular sequencing rule.

##### Example
1. Incremental message 1 (normal update): prevSeqId = 10, seqId = 15
2. Incremental message 2 (no update): prevSeqId = 15, seqId = 15
3. Incremental message 3 (sequence reset): prevSeqId = 15, seqId = 3
4. Incremental message 4 (normal update): prevSeqId = 3, seqId = 5

### SBE Order book

It is a public endpoint, returning SBE binary data for the initial 400 levels snapshot. This endpoint will buffer requests and return data only when a new snapshot is generated, approximately every 500 ms.<br><br>

Note: If the request fails, the error message will be provided in JSON format.<br><br>

For the HTTP request header, it doesn't need to be set to `application/sbe`; however, the response header will be `Content-Type`: `application/sbe` if the request is successful, and `Content-Type`: `application/json` if the request fails.

#### Rate Limit: 10 requests per 10 seconds
#### Rate limit rule: IP + instIdCode

#### HTTP Request

`GET /api/v5/market/books-sbe`

> Request Example

```shell
GET /api/v5/market/books-sbe?instIdCode=12345&source=0

```

#### Request Parameters

| Parameter   | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instIdCode | Integer | Yes      | Instruement ID code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| source      | Integer | Yes | The source of order book.<br>`0`: normal                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

> Response Example

```shell  
Error message example

Response header: 
Content-Type: application/json

Response body:
{
    "code": "51000",
    "msg": "Parameter instIdCode error",
    "data": []
}
```

#### Response Parameters
Please refer to the `SnapshotDepthResponseEvent` with ID `1006` in the XML schema.

### New error code

| Error Code | HTTP Status | Error Message |
| --- | --- | --- |
| 	60034	        | 	401	    | Only users who are {0} and above in trading fee tier are allowed to use this URL. |

### Upgrade
* In general, only compatible upgrades are made, such as adding a new field. In these cases, the XML schema ID remains unchanged, while the schema version is incremented.
* If a breaking change is needed, a new XML schema with a new schema ID will be released at least 1–2 months in advance. Before the end of the transition period, you’ll need to support both the old and new schemas, based on their schema ID and version.
