---
site: okex
lang: en
name: POST / Mass cancel order
method: POST
path: /api/v5/trade/mass-cancel
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Mass cancel order

Cancel all the MMP pending orders of an instrument family.<br>

Only applicable to Option in Portfolio Margin mode, and MMP privilege is required.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/mass-cancel`

> Request Example

```shell
POST /api/v5/trade/mass-cancel
body
{
    "instType":"OPTION",
    "instFamily":"BTC-USD"
}
```

#### Request Parameters

| Parameter | Type   | Required | Description                                             |
| :-------- | :----- |:---------|:--------------------------------------------------------|
| instType  | String | Yes       | Instrument type<br>`OPTION` |
| instFamily  | String | Yes        | Instrument family                                       |
| lockInterval | String | No | Lock interval(ms)<br> The range should be [0, 10 000]<br> The default is 0. You can set it as "0" if you want to unlock it immediately.<br> Error 54008 will be returned when placing order during lock interval, it is different from 51034 which is thrown when MMP is triggered                                                                                                                                                                                                                                                |

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
