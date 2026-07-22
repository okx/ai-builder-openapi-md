---
site: okex
lang: en
name: POST / Amend leading instruments
method: POST
path: /api/v5/copytrading/set-instruments
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend leading instruments

The leading trader can amend current leading instruments, need to set initial leading instruments while applying to become a leading trader.<br>
All non-leading instruments can't have position or pending orders for the current request when setting non-leading instruments as leading instruments.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`POST /api/v5/copytrading/set-instruments`

> Request example

```
POST /api/v5/copytrading/set-instruments
body
{
    "instId": "BTC-USDT-SWAP,ETH-USDT-SWAP"
}
```

#### Request Parameters

| Parameter    | Type   | Required | Description                                                                                                                                 |
| :-------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`, the default value                                                                                     |
| instId    | String | Yes       | Instrument ID, e.g. BTC-USDT-SWAP. If there are multiple instruments, separate them with commas. |

<aside class="notice">
The value of `instId` must include all instruments that you are going to have the lead trading with because the previous settings will be overwritten after the current request is set successfully<br>
</aside>

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
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter** | **Type** | **Description**                                        |
| :--------- |:---------|:-------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                      |
| enabled      | Boolean  | Whether you set it successfully<br/> `true` or `false` |
