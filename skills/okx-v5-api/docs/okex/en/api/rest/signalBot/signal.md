---
site: okex
lang: en
name: POST / Create signal
method: POST
path: /api/v5/tradingBot/signal/create-signal
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Create signal

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/create-signal`

> Request Example

```shell
POST /api/v5/tradingBot/signal/create-signal
body
{
  "signalChanName": "long short",
  "signalDesc": "this is the first version"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| signalChanName          | String          | Yes    | Signal channel name                                                                                                            |
| signalChanDesc          | String          | No     | Signal channel description                                                                                                            |

> Response Example

```json
{
    "code": "0",
    "data": [
       {
           "signalChanId" :"572112109",
           "signalChanToken":"dojuckew331lkx"
       }

    ],
    "msg": ""
}

```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| signalChanId      | String   | Signal channel Id                    |
| signalChanToken   | String   | User identify when placing orders via signal         |
