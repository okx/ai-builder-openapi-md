---
site: okex
lang: en
name: Get estimated future settlement price
method: GET
path: /api/v5/public/estimated-settlement-info
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get estimated future settlement price

Retrieve the estimated settlement price which will only have a return value one hour before the settlement.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/estimated-settlement-info`

> Request Example

```shell
GET /api/v5/public/estimated-settlement-info?instId=XRP-USDT-250307
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                            |
| :----- | :----- |:---------|:-----------------------------------------------------------------------|
| instId | String | Yes      | Instrument ID, e.g. `XRP-USDT-250307` <br>only applicable to `FUTURES` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "estSettlePx": "2.5666068562369959",
            "instId": "XRP-USDT-250307",
            "nextSettleTime": "1741248000000",
            "ts": "1741246429748"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                                                                   |
|:----------------| :------- |:----------------------------------------------------------------------------------|
| instId          | String   | Instrument ID, e.g. `XRP-USDT-250307`                                             |
| nextSettleTime  | String   | Next settlement time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| estSettlePx     | String   | Estimated settlement price                                                        |
| ts              | String   | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085`     |
