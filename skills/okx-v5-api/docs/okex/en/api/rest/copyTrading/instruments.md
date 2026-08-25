---
site: okex
lang: en
name: GET / Leading instruments
method: GET
path: /api/v5/copytrading/instruments
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Leading instruments

Retrieve instruments that are supported to lead by the platform. 
Retrieve instruments that the lead trader has set.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/instruments`

> Request example

```shell
GET /api/v5/copytrading/instruments

```

#### Request Parameters
| Parameter  | Type   | Required | Description                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`, the default value                                                                                                  |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "enabled": true,
            "instId": "BTC-USDT-SWAP"
        },
        {
            "enabled": true,
            "instId": "ETH-USDT-SWAP"
        },
        {
            "enabled": false,
            "instId": "ADA-USDT-SWAP"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                       |
| :---------- | :------- |:----------------------------------------------------------------------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                                                     |
| enabled      | Boolean   | Whether instrument is a lead instrument. `true` or `false`                                                            |
