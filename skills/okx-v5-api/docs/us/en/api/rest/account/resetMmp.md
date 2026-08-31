---
site: us
lang: en
name: Reset MMP Status
method: POST
path: /api/v5/account/mmp-reset
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Reset MMP Status

You can unfreeze by this endpoint once MMP is triggered.<br>

Only applicable to Option in Portfolio Margin mode, and MMP privilege is required.

<aside class="notice">
In the demo trading environment, MMP configurations may be periodically reset by the system. If your MMP status is unexpectedly reset in demo trading, please contact your BD manager or reach out to institutional@okx.com.
</aside>

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/mmp-reset`

> Request Example

```shell
POST /api/v5/account/mmp-reset
body
{
    "instType":"OPTION",
    "instFamily":"BTC-USD"
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                             |
| :-------- | :----- |:---------|:--------------------------------------------------------|
| instType  | String | No       | Instrument type<br>`OPTION`<br>The default is `OPTION |
| instFamily  | String | Yes      | Instrument family                                       |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "result":true
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                           |
| :-------- | :----- |:--------------------------------------|
| result     | Boolean | Result of the request `true`, `false` |
