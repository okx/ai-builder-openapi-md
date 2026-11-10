---
site: okex
lang: en
name: GET / Signal bot event history
method: GET
path: /api/v5/tradingBot/signal/event-history
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Signal bot event history

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/event-history`

> Request Example

```shell
GET /api/v5/tradingBot/signal/event-history?algoId=623833708424069120
```

#### Request Parameters

| Parameter  | Type     | Required | Description                                                                                                                                         |
|:-----------|:---------| :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId     | String   | Yes    | Algo ID                                                                                                                                             |
| after      | String   | No    | Pagination of data to return records `eventCtime` earlier than the requested timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| before     | String   | No    | Pagination of data to return records `eventCtime` newer than the requested timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
| limit      | String   | No    | Number of results per request. The maximum is 100. The default is 100.                                                                              |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "alertMsg": "{\"marketPosition\":\"short\",\"prevMarketPosition\":\"long\",\"action\":\"sell\",\"instrument\":\"ETHUSDT.P\",\"timestamp\":\"2023-10-16T10:50:00.000Z\",\"maxLag\":\"60\",\"investmentType\":\"base\",\"amount\":\"2\"}",
            "algoId": "623833708424069120",
            "eventCtime": "1697453400959",
            "eventProcessMsg": "Processed reverse entry signal and placed ETH-USDT-SWAP order with all available balance",
            "eventStatus": "success",
            "eventType": "signal_processing",
            "eventUtime": "",
            "triggeredOrdData": [
                {
                    "clOrdId": "O634100754731765763"
                },
                {
                    "clOrdId": "O634100754752737282"
                }
            ]
        }
     ],
     "msg": ""
}
```

#### Response Parameters

| **Parameter**     | **Type** | **Description**                                                                          |
|:------------------| :------- |:-----------------------------------------------------------------------------------------|
| alertMsg            | String                   | Alert message                                                                            |
| algoId            | String                   | Algo ID                                                                                  |
| eventType         | String                   | Event type<br>`system_action`<br>`user_action`<br>`signal_processing`                    |
| eventCtime        | String                   | Event timestamp of creation. Unix timestamp format in milliseconds, e.g. `1597026383085` |
| eventUtime        | String                   | Event timestamp of update. Unix timestamp format in milliseconds, e.g. `1597026383085`   |
| eventProcessMsg   | String                   | Event process message                                                                    |
| eventStatus       | String                   | Event status<br>`success`<br>`failure`                                                   |
| triggeredOrdData  | Array of objects          | Triggered sub order data                                                                 |
| > clOrdId         | String                   | Sub order client-supplied id                                                             |
