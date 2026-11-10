---
site: okex
lang: en
name: GET / Signals
method: GET
path: /api/v5/tradingBot/signal/signals
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Signals

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/signals`

> Request Example

```shell
GET /api/v5/tradingBot/signal/signals?signalSourceType=1
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| signalSourceType      | String          | Yes    | Signal source type<br>`1`: Created by yourself<br>`2`: Subscribe<br>`3`: Free signal    |
| signalChanId          | String          | No     | Signal channel id                                                       |
| after                 | String	      | No	   | Pagination of data to return records `signalChanId` earlier than the requested timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
| before	            | String	      | No	   | Pagination of data to return records `signalChanId` newer than the requested timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
| limit	                | String	      | No	   | Number of results per request. The maximum is 100. The default is 100.                                |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "signalChanId": "623833708424069120",
            "signalChanName": "test",
            "signalChanDesc": "test",
            "signalChanToken": "test",
            "signalSourceType": "1"
        }
    ],
    "msg": ""
}

```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| signalChanId      | String   | Signal channel id                     |
| signalChanName	| String   | Signal channel name                   |
| signalChanDesc	| String   | Signal channel description                   |
| signalChanToken   | String   | User identify when placing orders via signal         |
| signalSourceType  | String   | Signal source type<br>`1`: Created by yourself<br>`2`: Subscribe<br>`3`: Free signal    |
