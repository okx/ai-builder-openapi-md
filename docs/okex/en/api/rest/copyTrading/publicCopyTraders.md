---
site: okex
lang: en
name: GET / Copy traders
method: GET
path: /api/v5/copytrading/public-copy-traders
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Copy traders

Public endpoint. Retrieve copy trader coming from certain lead trader. Return according to `pnl` from high to low

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-copy-traders`

> Request example

```shell
GET /api/v5/copytrading/public-copy-traders?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                   |
| :------ | :----- |:---------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                 |
| uniqueCode  | String | Yes      | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters) |
| limit    | String | No       | Number of results per request. The maximum is `100`; The default is `100`           |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "copyTotalPnl": "2060.12242",
            "copyTraderNumChg": "1",
            "copyTraderNumChgRatio": "0.5",
            "copyTraders": [
                {
                    "beginCopyTime": "1686125051000",
                    "nickName": "bre***@gmail.com",
                    "pnl": "1076.77388",
                    "portLink": ""
                },
                {
                    "beginCopyTime": "1698133811000",
                    "nickName": "MrYanDao505",
                    "pnl": "983.34854",
                    "portLink": "https://static.okx.com/cdn/okex/users/headimages/20231010/fd31f45e99fe41f7bb219c0b53ae0ada"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**         | **Type** | **Description**                                                              |
|:----------------------|:---------|:-----------------------------------------------------------------------------|
| copyTotalPnl          | String   | Total copy trader profit and loss                                                        |
| ccy                   | String   | The currency name of profit and loss                                                     |
| copyTraderNumChg      | String   | Number change in last 7 days                                                 |
| copyTraderNumChgRatio | String   | Ratio change in last 7 days                                                  |
| copyTraders           | Array of objects    | Copy trader information                                                      |
| > beginCopyTime       | String   | Begin copying time. Unix timestamp format in milliseconds, e.g.1597026383085 |
| > nickName            | String   | Nick name                                                                    |
| > portLink            | String   | Copy trader portrait link                                                    |
| > pnl                 | String   | Copy trading profit and loss                                                 |
