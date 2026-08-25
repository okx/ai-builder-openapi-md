---
site: turkey
lang: en
name: WS / Candlesticks channel
type: guide
---

### WS / Candlesticks channel

Retrieve the candlesticks data of an instrument. the push frequency is the fastest interval 1 second push the data.

#### URL Path

/ws/v5/business

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "candle1D",
      "instId": "BTC-USDT"
    }
  ]
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                 |
|:----------| :----- | :------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`                                                                                                                                                                                                                                                                                                                                   |
| args      | Array of objects  | Yes      | List of subscribed channels                                                                                                                                                                                                                                                                                                                                                 |
| > channel   | String | Yes      | Channel name<br>`candle3M`<br>`candle1M`<br>`candle1W`<br>`candle1D`<br>`candle2D`<br>`candle3D`<br>`candle5D`<br>`candle12H`<br>`candle6H`<br>`candle4H`<br>`candle2H`<br>`candle1H`<br>`candle30m`<br>`candle15m`<br>`candle5m`<br>`candle3m`<br>`candle1m`<br>`candle1s`<br>`candle3Mutc`<br>`candle1Mutc`<br>`candle1Wutc`<br>`candle1Dutc`<br>`candle2Dutc`<br>`candle3Dutc`<br>`candle5Dutc`<br>`candle12Hutc`<br>`candle6Hutc` |
| > instId    | String | Yes      | Instrument ID                                                                                                                                                                                                                                                                                                                                                               |

> Successful Response Example

```json
{
  "op": "subscribe",
  "event": "subscribe",
  "arg": {
    "channel": "candle1D",
    "instId": "BTC-USDT"
  },
  "connId": "a4d3ae55"
}
```

> Failure Response Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"candle1D\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type     | Required | Description                                      |
|:----------|:---------|:---------|--------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String   | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | No       | Subscribed channel                               |
| > channel   | String   | yes      | channel name                                     |
| > instId    | String   | Yes      | Instrument ID                                    |
| code      | String   | No       | Error code                                       |
| msg       | String   | No       | Error message                                    |
| connId    | String   | Yes      | WebSocket connection ID                          |

> Push Data Example

```json
{
  "arg": {
    "channel": "candle1D",
    "instId": "BTC-USDT"
  },
  "data": [
    [
      "1597026383085",
      "8533.02",
      "8553.74",
      "8527.17",
      "8548.26",
      "45247",
      "529.5858061",
      "5529.5858061",
      "0"
    ]
  ]
}
```

#### Push data parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                 |
|:--------------| :------- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg           | Object   | Successfully subscribed channel                                                                                                                                                                 |
| > channel     | String   | Channel name                                                                                                                                                                                    |
| > instId      | String   | Instrument ID                                                                                                                                                                                   |
| data          | Array of Arrays    | Subscribed data                                                                                                                                                                                 |
| > ts          | String   | Opening time of the candlestick, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                    |
| > o           | String   | Open price                                                                                                                                                                                      |
| > h           | String   | highest price                                                                                                                                                                                   |
| > l           | String   | Lowest price                                                                                                                                                                                    |
| > c           | String   | Close price                                                                                                                                                                                     |
| > vol         | String   | Trading volume<br>If it is `SPOT`, the value is the quantity in base currency.   |
| > volCcy      | String   | Trading volume<br>If it is `SPOT`, the value is the quantity in quote currency. |
| > volCcyQuote | String  | Trading volume, the value is the quantity in quote currency <br>e.g. The unit is `USDT` for `BTC-USDT`                                        |
| > confirm     | String  | The state of candlesticks<br>`0`: K line is uncompleted<br>`1`: K line is completed                                                                                                             |
