---
site: okex
lang: en
name: POST / Place lead stop order
method: POST
path: /api/v5/copytrading/algo-order
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Place lead stop order

Set TP/SL for the current lead position that are not closed.

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/copytrading/algo-order`

> Request example

```
POST /api/v5/copytrading/algo-order
body
{
    "subPosId": "518541406042591232",
    "tpTriggerPx": "10000"
}
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                   |
| :------ | :----- |:-----|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`, the default value                                                                                                                                                       |
| subPosId  | String | Yes    | Lead position ID                                                                                                                                                                                      |
| tpTriggerPx  | String | Conditional    | Take-profit trigger price. Take-profit order price will be the market price after triggering. At least one of tpTriggerPx and slTriggerPx must be filled<br>The take profit order will be deleted if it is 0 |
| slTriggerPx  | String | Conditional    | Stop-loss trigger price. Stop-loss order price will be the market price after triggering. The stop loss order will be deleted if it is 0                                                                      |
| tpOrdPx     | String | No       | Take-profit order price<br>If the price is -1, take-profit will be executed at the market price, the default is `-1`<br>Only applicable to `SPOT` lead trader                                                |
| slOrdPx     | String | No       | Stop-loss order price<br>If the price is -1, stop-loss will be executed at the market price, the default is `-1`<br>Only applicable to `SPOT` lead trader                                                    |
| tpTriggerPxType  | String | No    | Take-profit trigger price type <br><br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>Default is `last`                                                                               |
| slTriggerPxType  | String | No    | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price <br>Default is last                                                                                    |
| tag       | String | No      | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                              |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "subPosId": "518560559046594560",
            "tag":""
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subPosId      | String   | Lead position ID                                                                                                                                                                                         |
| tag      | String   | Order tag                                                                                                                                                                                      |
