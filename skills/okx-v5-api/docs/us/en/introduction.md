---
site: us
lang: en
name: Overview
type: guide
---

# Overview

Welcome to our API documentation. OKX provides REST and WebSocket APIs to suit your trading needs.<br>

<aside class="warning">
<b>Important - OKX API Agreement: continued use constitutes acceptance</b><br><br>

We encourage you to review the <a href="https://www.okx.com/help/okx-api-agreement">OKX API Agreement</a> in full before continuing to use the API Services. Your continued use of any OKX API Services following the date of this notice constitutes your acceptance of the Agreement in full. <br>

If you do not agree to the Agreement, you must immediately cease all use of the OKX API Services, including disconnecting any active API integrations, Agent Trade Kit connections, and automated systems that access OKX via APIs.
</aside>

## API Resources and Support

### Tutorials
- Learn how to trade with API: [Best practice to OKX’s API](/docs-v5/trick_en/#instrument-configuration)
- Learn python spot trading step by step: [Python Spot Trading Tutorial](/help/how-can-i-do-spot-trading-with-the-jupyter-notebook)
- Learn python derivatives trading step by step: [Python Derivatives Trading Tutorial](/help/how-can-i-do-derivatives-trading-with-the-jupyter-notebook)

<br>

### Python libraries
- Use Python SDK for easier integration: [Python SDK](https://pypi.org/project/python-okx/)
- Get access to our market maker python sample code [Python market maker sample](https://github.com/okxapi/okx-sample-market-maker)

<br>

### Customer service

- Please take 1 minute to help us improve: [API Satisfaction Survey](https://forms.gle/Ehou2xFv5GE1xUGr9)
- If you have any API-related inquiries, you can reach out to us by scanning the code below via the OKX APP.

<p><img src="images/APISupport.jpeg" alt="API Technical Support QR Code" style="max-width: 300px; display: block; margin: 20px auto;" /></p>

## API key Creation

Please refer to [my api page](/account/my-api) regarding API Key creation.

### Generating an API key

Create an API key on the website before signing any requests. After creating an API key, keep the following information safe:

- API key
- Secret key
- Passphrase

The system returns randomly-generated API keys and SecretKeys. You will need to provide the Passphrase to access the API. We store the salted hash of your Passphrase for authentication. We cannot recover the Passphrase if you have lost it. You will need to create a new set of API key.<br><br>

### API key permissions

There are three permissions below that can be associated with an API key. One or more permission can be assigned to any key.<br>

- `Read` : Can request and view account info such as bills and order history which need read permission
- `Trade` : Can place and cancel orders, funding transfer, make settings which need write permission
- `Withdraw` : Can make withdrawals

### API key security

<aside class="warning">
To improve security, we strongly recommend clients linked the API key to IP addresses<br>
</aside>

- Each API key can bind up to 20 IP addresses, which support IPv4/IPv6 and network segment formats.
<br>

<aside class="notice">
API keys that are not linked to an IP address and have `trade` or `withdraw` permissions will expire after 14 days of inactivity. (The API key of demo trading will not expire) <br>
</aside>

- Only when the user calls an API that requires API key authentication will it be considered as the API key is used.
- Calling an API that does not require API key authentication will not be considered used even if API key information is passed in.
- For websocket, only operation of logging in will be considered to have used the API key. Any operation though the connection after logging in (such as subscribing/placing an order) will not be considered to have used the API key. Please pay attention.

Users can get the usage records of the API key with `trade` or `withdraw` permissions but unlinked to any IP address though [Security Center](/account/security).

## REST Authentication

### Making Requests

All private REST requests must contain the following headers:

- `OK-ACCESS-KEY` The API key as a String.

- `OK-ACCESS-SIGN` The Base64-encoded signature (see Signing Messages subsection for details).

- `OK-ACCESS-TIMESTAMP` Request timestamp in ISO 8601 UTC format with millisecond precision, e.g. `2020-12-08T09:08:57.715Z`. The server rejects requests where this differs from server time by more than 30 seconds (error 50102). Always use UTC — local timezone offset is the most common cause of error 50102. Synchronise with GET /api/v5/public/time before placing orders.

- `OK-ACCESS-PASSPHRASE` The passphrase you specified when creating the API key.

Request bodies should have content type `application/json` and be in valid JSON format.

### Signature

> Signing Messages

The `OK-ACCESS-SIGN` header is generated as follows:

- Create a pre-hash string of timestamp + method + requestPath + body (where + represents String concatenation).
- Prepare the SecretKey.
- Sign the pre-hash string with the SecretKey using the HMAC SHA256.
- Encode the signature in the Base64 format.

Example: `sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp + 'GET' + '/api/v5/account/balance?ccy=BTC', SecretKey))`

The `timestamp` value is the same as the `OK-ACCESS-TIMESTAMP` header with millisecond ISO format, e.g. `2020-12-08T09:08:57.715Z`.

The request method should be in UPPERCASE: e.g. `GET` and `POST`.

The `requestPath` is the path of requesting an endpoint.

Example: `/api/v5/account/balance`

The `body` refers to the String of the request body. It can be omitted if there is no request body (frequently the case for `GET` requests).

Example: `{"instId":"BTC-USDT","lever":"5","mgnMode":"isolated"}`

<aside class="notice">
`GET` request parameters are counted as requestpath, not body
</aside>

The SecretKey is generated when you create an API key.

Example: `22582BD0CFF14C41EDBF1AB98506286D`

## WebSocket

### Overview

WebSocket is a new HTML5 protocol that achieves full-duplex data transmission between the client and server, allowing data to be transferred effectively in both directions. A connection between the client and server can be established with just one handshake. The server will then be able to push data to the client according to preset rules. Its advantages include:

- The WebSocket request header size for data transmission between client and server is only 2 bytes.
- Either the client or server can initiate data transmission.
- There's no need to repeatedly create and delete TCP connections, saving resources on bandwidth and server.

<aside class="notice">
We recommend developers use WebSocket API to retrieve market data and order book depth.
</aside>

### Connect

**Connection limit**: 3 requests per second (based on IP)

When subscribing to a public channel, use the address of the public service. When subscribing to a private channel, use the address of the private service

**Request limit**: 

The total number of 'subscribe'/'unsubscribe'/'login' requests per connection is limited to 480 times per hour.

<aside class="notice">
<p>If there’s a network problem, the system will automatically disable the connection.
<p>The connection will break automatically if the subscription is not established or data has not been pushed for more than 30 seconds.
<p>To keep the connection stable: 
<p>1. Set a timer of N seconds whenever a response message is received, where N is less than 30. 
<p>2. If the timer is triggered, which means that no new message is received within N seconds, send the String 'ping'.
<p>3. Expect a 'pong' as a response. If the response message is not received within N seconds, please raise an error or reconnect.
</aside>

### Connection count limit
The limit will be set at 30 WebSocket connections per specific WebSocket channel per sub-account. Each WebSocket connection is identified by the unique `connId`.

<br>

The WebSocket channels subject to this limitation are as follows:

1. [Orders channel](/docs-v5/en/#order-book-trading-trade-ws-order-channel)
2. [Account channel](/docs-v5/en/#trading-account-websocket-account-channel)
3. [Positions channel](/docs-v5/en/#trading-account-websocket-positions-channel)
4. [Balance and positions channel](/docs-v5/en/#trading-account-websocket-balance-and-position-channel)
5. [Position risk warning channel](/docs-v5/en/#trading-account-websocket-position-risk-warning)
6. [Account greeks channel](/docs-v5/en/#trading-account-websocket-account-greeks-channel)

If users subscribe to the same channel through the same WebSocket connection through multiple arguments, for example, by using `{"channel": "orders", "instType": "ANY"}` and `{"channel": "orders", "instType": "SWAP"}`, it will be counted once only. If users subscribe to the listed channels (such as orders and accounts) using either the same or different connections, it will not affect the counting, as these are considered as two different channels. The system calculates the number of WebSocket connections per channel.

<br>

The platform will send the number of active connections to clients through the `channel-conn-count` event message <b>to new channel subscriptions</b>.

> Connection count update

```json
{
    "event":"channel-conn-count",
    "channel":"orders",
    "connCount": "2",
    "connId":"abcd1234"
}

```

<br>

When the limit is breached, generally the latest connection that sends the subscription request will be rejected. Client will receive the usual subscription acknowledgement followed by the `channel-conn-count-error` from the connection that the subscription has been terminated. In exceptional circumstances the platform may unsubscribe existing connections.

> Connection limit error

```json
{
    "event": "channel-conn-count-error",
    "channel": "orders",
    "connCount": "30",
    "connId":"a4d3ae55"
}

```

<br>

Order operations through WebSocket, including place, amend and cancel orders, are not impacted through this change.

### Login

> Request Example

```shell
{
  "op": "login",
  "args": [
    {
      "apiKey": "******",
      "passphrase": "******",
      "timestamp": "1538054050",
      "sign": "7L+zFQ+CEgGu5rzCj4+BdV2/uUHGqddA9pI6ztsRRPs="
    }
  ]
}
```

#### Request Parameters

| Parameter    | Type             | Required   | Description                          |
|:-------------|:-----------------|:-----------|--------------------------------------|
| op           | String           | Yes        | Operation<br>`login`                 |
| args         | Array of objects | Yes        | List of account to login             |
| > apiKey     | String           | Yes        | API Key                              |
| > passphrase | String           | Yes        | API Key password                     |
| > timestamp  | String           | Yes        | Unix Epoch time, the unit is seconds |
| > sign       | String           | Yes        | Signature string                     |

> Successful Response Example

```json
{
  "event": "login",
  "code": "0",
  "msg": "",
  "connId": "a4d3ae55"
}
```

> Failure Response Example

```json
{
  "event": "error",
  "code": "60009",
  "msg": "Login failed.",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                     |
| :-------- | :----- | :------- |---------------------------------|
| event     | String | Yes      | Operation<br>`login`<br>`error` |
| code      | String | No       | Error code                      |
| msg       | String | No       | Error message                   |
| connId   | String   | Yes  | WebSocket connection ID         |

**apiKey**: Unique identification for invoking API. Requires user to apply one manually.

**passphrase**: API Key password

**timestamp**: the Unix Epoch time, the unit is seconds, e.g. 1704876947

**sign**: signature string, the signature algorithm is as follows:

First concatenate `timestamp`, `method`, `requestPath`, strings, then use HMAC SHA256 method to encrypt the concatenated string with SecretKey, and then perform Base64 encoding.

**secretKey**: The security key generated when the user applies for API key, e.g. `22582BD0CFF14C41EDBF1AB98506286D`

**Example of timestamp**: const timestamp = '' + Date.now() / 1,000

**Among sign example**: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp +'GET'+'/users/self/verify', secretKey))

**method**: always 'GET'.

**requestPath** : always '/users/self/verify'

<aside class="notice">
The request will expire 30 seconds after the timestamp. If your server time differs from the API server time, we recommended using the REST API to query the API server time and then set the timestamp.
</aside>

### Subscribe

**Subscription Instructions**

> Request format description

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": ["<SubscriptionTopic>"]
}
```

WebSocket channels are divided into two categories: `public` and `private` channels.

`Public channels` -- No authentication is required, include tickers channel, K-Line channel, limit price channel, order book channel, and mark price channel etc.

`Private channels` -- including account channel, order channel, and position channel, etc -- require log in.

Users can choose to subscribe to one or more channels, and the total length of multiple channels cannot exceed 64 KB.

Below is an example of subscription parameters. The requirement of subscription parameters for each channel is different. For details please refer to the specification of each channels.

> Request Example

```shell
{
    "id": "1512",
    "op":"subscribe",
    "args":[
        {
            "channel":"tickers",
            "instId":"BTC-USDT"
        }
    ]
}
```

**Request parameters**

| Parameter    | Type   | Required | Description                                                                         |
|:-------------| :----- | :------- |-------------------------------------------------------------------------------------|
| id           | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op           | String | Yes      | Operation<br>`subscribe`                                                            |
| args         | Array of objects  | Yes      | List of subscribed channels                                                         |
| > channel    | String | Yes      | Channel name                                                                        |
| > instType   | String | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`ANY`   |
| > instFamily | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                        |
| > instId     | String | No       | Instrument ID                                                                       |

> Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "accb8e21"
}
```

**Return parameters**

| Parameter    | Type     | Required | Description                                                                       |
|:-------------|:---------|:---------|-----------------------------------------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                                                  |
| event        | String   | Yes      | Event<br>`subscribe`<br>`error`                                                   |
| arg          | Object   | No       | Subscribed channel                                                                |
| > channel    | String   | Yes      | Channel name                                                                      |
| > instType   | String   | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`ANY` |
| > instFamily | String   | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                      |
| > instId     | String   | No       | Instrument ID                                                                     |
| code         | String   | No       | Error code                                                                        |
| msg          | String   | No       | Error message                                                                     |
| connId       | String   | Yes      | WebSocket connection ID                                                           |

### Unsubscribe

Unsubscribe from one or more channels.

> Request format description

```shell
{
  "op": "unsubscribe",
  "args": ["< SubscriptionTopic> "]
}
```

> Request Example

```shell
{
  "op": "unsubscribe",
  "args": [
    {
      "channel": "tickers",
      "instId": "BTC-USDT"
    }
  ]
}
```

**Request parameters**

| Parameter    | Type   | Required | Description                                                                         |
|:-------------| :----- | :------- |-------------------------------------------------------------------------------------|
| op           | String | Yes      | Operation<br>`unsubscribe`                                                          |
| args         | Array of objects  | Yes      | List of channels to unsubscribe from                                                |
| > channel    | String | Yes      | Channel name                                                                        |
| > instType   | String | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`ANY`   |
| > instFamily | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                        |
| > instId     | String | No       | Instrument ID                                                                       |

> Response Example

```json
{
    "event": "unsubscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "d0b44253"
}
```

**Response parameters**

| Parameter    | Type   | Required | Description                                                               |
|:-------------| :----- | :------- |---------------------------------------------------------------------------|
| event        | String | Yes      | Event<br>`unsubscribe`<br>`error`                                         |
| arg          | Object | No       | Unsubscribed channel                                                      |
| > channel    | String | Yes      | Channel name                                                              |
| > instType   | String | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION ` |
| > instFamily | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`              |
| > instId     | String | No       | Instrument ID                                                             |
| code         | String | No       | Error code                                                                |
| msg          | String | No       | Error message                                                             |

### Notification

WebSocket has introduced a new message type (event = `notice`). <br><br>

Client will receive the information in the following scenarios:

- Websocket disconnect for service upgrade<br>

60 seconds prior to the upgrade of the WebSocket service, the notification message will be sent to users indicating that the connection will soon be disconnected.
Users are encouraged to establish a new connection to prevent any disruptions caused by disconnection.

> Response Example

```json
{
    "event": "notice",
    "code": "64008",
    "msg": "The connection will soon be closed for a service upgrade. Please reconnect.",
    "connId": "a4d3ae55"
}
```
<br><br>
The feature is supported by WebSocket Public (/ws/v5/public) and Private (/ws/v5/private) for now.

## Account mode
To facilitate your trading experience, please set the appropriate account mode before starting trading.

In the trading account trading system, 4 account modes are supported: `Spot mode`, `Futures mode`, `Multi-currency margin mode`, and `Portfolio margin mode`.

You need to set on the Web/App for the first set of every account mode.

## Production Trading Services

The Production Trading URL:  

- REST: `https://us.okx.com`  
- Public WebSocket: `wss://wsus.okx.com:8443/ws/v5/public`  
- Private WebSocket: `wss://wsus.okx.com:8443/ws/v5/private`
- Business WebSocket: `wss://wsus.okx.com:8443/ws/v5/business`

## Demo Trading Services  

Currently, the API works for Demo Trading, but some functions are not supported, such as `withdraw`,`deposit`,`purchase/redemption`, etc.

The Demo Trading URL:  

- REST: `https://us.okx.com`  
- Public WebSocket: `wss://wsuspap.okx.com:8443/ws/v5/public`  
- Private WebSocket: `wss://wsuspap.okx.com:8443/ws/v5/private`  
- Business WebSocket: `wss://wsuspap.okx.com:8443/ws/v5/business`

OKX account can be used for login on Demo Trading. If you already have an OKX account, you can log in directly.

Start API Demo Trading by the following steps:  
Login OKX —> Trade —> Demo Trading —> Personal Center —> Demo Trading API -> Create Demo Trading API Key —> Start your Demo Trading

<aside class="notice">
Note: `x-simulated-trading: 1` needs to be added to the header of the Demo Trading request.
</aside>

> Http Header Example  

```wiki
Content-Type: application/json

OK-ACCESS-KEY: 37c541a1-****-****-****-10fe7a038418

OK-ACCESS-SIGN: leaVRETrtaoEQ3yI9qEtI1CZ82ikZ4xSG5Kj8gnl3uw=

OK-ACCESS-PASSPHRASE: 1****6

OK-ACCESS-TIMESTAMP: 2020-03-28T12:21:41.274Z

x-simulated-trading: 1
```

## Transaction Timeouts

Orders may not be processed in time due to network delay or busy OKX servers. You can configure the expiry time of the request using `expTime` if you want the order request to be discarded after a specific time.

If `expTime` is specified in the requests for Place (multiple) orders or Amend (multiple) orders, the request will not be processed if the current system time of the server is after the `expTime`.

### REST API

Set the following parameters in the request header

| Parameter | Type   | Required | Description                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| expTime | String | No     | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085` |

The following endpoints are supported:

- [Place order](/docs-v5/en/#order-book-trading-trade-post-place-order)
- [Place multiple orders](/docs-v5/en/#order-book-trading-trade-post-place-multiple-orders)
- [Amend order](/docs-v5/en/#order-book-trading-trade-post-amend-order)
- [Amend multiple orders](/docs-v5/en/#order-book-trading-trade-post-amend-multiple-orders)
- [POST / Place sub order](/docs-v5/en/#order-book-trading-signal-bot-trading-post-place-sub-order) under signal bot trading

> Request Example

```wiki
curl -X 'POST' \
  'https://openapi.okx.com/api/v5/trade/order' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'OK-ACCESS-KEY: *****' \
  -H 'OK-ACCESS-SIGN: *****'' \
  -H 'OK-ACCESS-TIMESTAMP: *****'' \
  -H 'OK-ACCESS-PASSPHRASE: *****'' \
  -H 'expTime: 1597026383085' \   // request effective deadline
  -d '{
  "instId": "BTC-USDT",
  "tdMode": "cash",
  "side": "buy",
  "ordType": "limit",
  "px": "1000",
  "sz": "0.01"
}'
```

### WebSocket

The following parameters are set in the request

| Parameter | Type   | Required | Description                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| expTime | String | No     | Request effective deadline. Unix timestamp format in milliseconds, e.g. `1597026383085` |

The following endpoints are supported:

- [Place order](/docs-v5/en/#order-book-trading-trade-ws-place-order)
- [Place multiple orders](/docs-v5/en/#order-book-trading-trade-ws-place-multiple-orders)
- [Amend order](/docs-v5/en/#order-book-trading-trade-ws-amend-order)
- [Amend multiple orders](/docs-v5/en/#order-book-trading-trade-ws-amend-multiple-orders)

> Request Example

```wiki
{
    "id": "1512",
    "op": "order",
    "expTime":"1597026383085",  // request effective deadline
    "args": [{
        "side": "buy",
        "instId": "BTC-USDT",
        "tdMode": "isolated",
        "ordType": "market",
        "sz": "100"
    }]
}
```

## Rate Limits

Our REST and WebSocket APIs use rate limits to protect our APIs against malicious usage so our trading platform can operate reliably and fairly.<br>
When a request is rejected by our system due to rate limits, the system returns error code 50011 (Rate limit reached. Please refer to API documentation and throttle requests accordingly).<br>
The rate limit is different for each endpoint. You can find the limit for each endpoint from the endpoint details. Rate limit definitions are detailed below:

- WebSocket login and subscription rate limits are based on connection.

- Public unauthenticated REST rate limits are based on IP address.

- Private REST rate limits are based on User ID (sub-accounts have individual User IDs).

- WebSocket order management rate limits are based on User ID (sub-accounts have individual User IDs).

### Trading-related APIs
For Trading-related APIs (place order, cancel order, and amend order) the following conditions apply:

- Rate limits are shared across the REST and WebSocket channels. 

- Rate limits for placing orders, amending orders, and cancelling orders are independent from each other. 

- Rate limits are defined on the Instrument ID level (except Options)

- Rate limits for Options are defined based on the Instrument Family level. Refer to the [Get instruments](/docs-v5/en/#public-data-rest-api-get-instruments) endpoint to view Instrument Family information.

- Rate limits for a multiple order endpoint and a single order endpoint are also independent, with the exception being when there is only one order sent to a multiple order endpoint, the order will be counted as a single order and adopt the single order rate limit. 

### Sub-account rate limit

At the sub-account level, we allow a maximum of 1000 order requests per 2 seconds. Only new order requests and amendment order requests will be counted towards this limit. The limit encompasses all requests from the endpoints below. For batch order requests consisting of multiple orders, each order will be counted individually. Error code 50061 is returned when the sub-account rate limit is exceeded. The existing rate limit rule per instrument ID remains unchanged and the existing rate limit and sub-account rate limit will operate in parallel. If clients require a higher rate limit, clients can trade via multiple sub-accounts.

* [POST / Place order](/docs-v5/en/#order-book-trading-trade-post-place-order)
* [POST / Place multiple orders](/docs-v5/en/#order-book-trading-trade-post-place-multiple-orders)
* [POST / Amend order](/docs-v5/en/#order-book-trading-trade-post-amend-order)
* [POST / Amend multiple orders](/docs-v5/en/#order-book-trading-trade-post-amend-multiple-orders)

* [WS / Place order](/docs-v5/en/#order-book-trading-trade-ws-place-order)
* [WS / Place multiple orders](/docs-v5/en/#order-book-trading-trade-ws-place-multiple-orders)
* [WS / Amend order](/docs-v5/en/#order-book-trading-trade-ws-amend-order)
* [WS / Amend multiple orders](/docs-v5/en/#order-book-trading-trade-ws-amend-multiple-orders)
<br>

### Fill ratio based sub-account rate limit 
This is only applicable to >= VIP5 customers.
<br>
As an incentive for more efficient trading, the exchange will offer a higher sub-account rate limit to clients with a high trade fill ratio. 
<br><br>
The exchange calculates two ratios based on the transaction data from the past 7 days at 00:00 UTC.

1. Sub-account fill ratio: This ratio is determined by dividing (the trade volume in USDT of the sub-account) by (sum of (new and amendment request count per symbol * symbol multiplier) of the sub-account). Note that the master trading account itself is also considered as a sub-account in this context.
2. Master account aggregated fill ratio: This ratio is calculated by dividing (the trade volume in USDT on the master account level) by (the sum (new and amendment count per symbol * symbol multiplier] of all sub-accounts).

<br>

The symbol multiplier allows for fine-tuning the weight of each symbol. A smaller symbol multiplier (<1) is used for smaller pairs that require more updates per trading volume. All instruments have a default symbol multiplier, and some instruments will have overridden symbol multipliers.

| InstType            | Override rule         | Overridden symbol multiplier                                                                 | Default symbol multiplier |
|---------------------|-----------------------|----------------------------------------------------------------------------------------------|---------------------------|
| Perpetual Futures   | Per instrument ID     | `1` <br>Instrument ID: <br>BTC-USDT-SWAP <br>BTC-USD-SWAP <br>ETH-USDT-SWAP <br>ETH-USD-SWAP | `0.2`                     |
| Expiry Futures      | Per instrument Family | `0.3` <br>Instrument Family: <br>BTC-USDT <br>BTC-USD <br>ETH-USDT <br>ETH-USD               | `0.1`                     |
| Spot                | Per instrument ID     | `0.5` <br>Instrument ID: <br>BTC-USDT <br>ETH-USDT                                           | `0.1`                     |
| Options             | Per instrument Family |                                                                                              | `0.1`                     |

The fill ratio computation excludes block trading, spread trading, MMP and fiat orders for order count; and excludes block trading, spread trading for trade volume. Only successful order requests (sCode=0) are considered.

<br><br>

At 08:00 UTC, the system will use the maximum value between the sub-account fill ratio and the master account aggregated fill ratio based on the data snapshot at 00:00 UTC to determine the sub-account rate limit based on the table below. For broker (non-disclosed) clients, the system considers the sub-account fill ratio only.

|          | Fill ratio[x<=ratio<y) | Sub-account rate limit per 2 seconds(new and amendment) |
|----------|------------------------|---------------------------------------------------------|
| Tier 1   | [0,1)                  | 1,000                                                   |
| Tier 2   | [1,2)                  | 1,250                                                   |
| Tier 3   | [2,3)                  | 1,500                                                   |
| Tier 4   | [3,5)                  | 1,750                                                   |
| Tier 5   | [5,10)                 | 2,000                                                   |
| Tier 6   | [10,20)                | 2,500                                                   |
| Tier 7   | [20,50)                | 3,000                                                   |
| Tier 8   | >= 50                  | 10,000                                                  |

If there is an improvement in the fill ratio and rate limit to be uplifted, the uplift will take effect immediately at 08:00 UTC. However, if the fill ratio decreases and the rate limit needs to be lowered, a one-day grace period will be granted, and the lowered rate limit will only be implemented on T+1 at 08:00 UTC. On T+1, if the fill ratio improves, the higher rate limit will be applied accordingly. In the event of client demotion to VIP4, their rate limit will be downgraded to Tier 1, accompanied by a one-day grace period.

<br>

If the 7-day trading volume of a sub-account is less than 1,000,000 USDT, the fill ratio of the master account will be applied to it.

<br>

For newly created sub-accounts, the Tier 1 rate limit will be applied at creation until T+1 8am UTC, at which the normal rules will be applied.

<br>

Block trading, spread trading, MMP and spot/margin orders are exempted from the sub-account rate limit.

<br>

The exchange offers [GET / Account rate limit] (/docs-v5/en/#order-book-trading-trade-get-account-rate-limit) endpoint that provides ratio and rate limit data, which will be updated daily at 8am UTC. It will return the sub-account fill ratio, the master account aggregated fill ratio, current sub-account rate limit and sub-account rate limit on T+1 (applicable if the rate limit is going to be demoted).
<br><br>
The fill ratio and rate limit calculation example is shown below. Client has 3 accounts, symbol multiplier for BTC-USDT-SWAP = 1 and XRP-USDT = 0.1.

1. Account A (master account):
    1. BTC-USDT-SWAP trade volume = 100 USDT, order count = 10;
    2. XRP-USDT trade volume = 20 USDT, order count = 15;
    3. Sub-account ratio = (100+20) / (10 * 1 + 15 * 0.1) = 10.4
2. Account B (sub-account):
    1. BTC-USDT-SWAP trade volume = 200 USDT, order count = 100;
    2. XRP-USDT trade volume = 20 USDT, order count = 30;
    3. Sub-account ratio = (200+20) / (100 * 1 + 30 * 0.1) = 2.13
3. Account C (sub-account):
    1. BTC-USDT-SWAP trade volume = 300 USDT, order count = 1000;
    2. XRP-USDT trade volume = 20 USDT, order count = 45;
    3. Sub-account ratio = (300+20) / (100 * 1 + 45 * 0.1) = 3.06
4. Master account aggregated fill ratio = (100+20+200+20+300+20) / (10 * 1 + 15 * 0.1 + 100 * 1 + 30 * 0.1 + 100 * 1 + 45 * 0.1) = 3.01
5. Rate limit of accounts
    1. Account A = max(10.4, 3.01) = 10.4 -> 2500 order requests/2s
    2. Account B = max(2.13, 3.01) = 3.01 -> 1750 order requests/2s
    3. Account C = max(3.06, 3.01) = 3.06 -> 1750 order requests/2s

### Best practices
If you require a higher request rate than our rate limit, you can set up different sub-accounts to batch request rate limits. We recommend this method for throttling or spacing out requests in order to maximize each accounts' rate limit and avoid disconnections or rejections.
