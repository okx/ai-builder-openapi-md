---
site: okex
lang: en
name: Cancel All After
method: POST
path: /api/v5/rfq/cancel-all-after
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel All After

Cancel all quotes after the countdown timeout.

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/rfq/cancel-all-after`

> Request Example

```shell
POST /api/v5/rfq/cancel-all-after
body
{
   "timeOut":"60"
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                |
| :-------- | :----- |:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| timeOut  | String | Yes       | The countdown for quotes cancellation, with second as the unit.<br>Range of value can be 0, [10, 120]. <br>Setting timeOut to 0 disables Cancel All After. |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "ts":"1587971400"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                           |
| :-------- | :----- |:--------------------------------------|
| triggerTime     | String | The time the cancellation is triggered.<br>triggerTime=0 means Cancel All After is disabled. |
| ts     | String | The time the request is received. |

<aside class="notice">
Users are recommended to send a request to the exchange every second. When the cancel all after is triggered, the trading engine will cancel quotes on behalf of the client one by one and this operation may take up to a few seconds. This feature is intended as a protection mechanism for clients only and clients should not use this feature as part of their trading strategies.
</aside>
