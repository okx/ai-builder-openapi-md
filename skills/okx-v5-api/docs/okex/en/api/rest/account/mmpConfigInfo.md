---
site: okex
lang: en
name: GET MMP Config
method: GET
path: /api/v5/account/mmp-config
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET MMP Config

This endpoint is used to get MMP configure information<br>

Only applicable to Option in Portfolio Margin mode, and MMP privilege is required.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/mmp-config`

> Request Example

```shell
GET /api/v5/account/mmp-config?instFamily=BTC-USD

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                       |
| :------ | :----- | :------- | :----------------------------------------- |
| instFamily  | String | No       | Instrument Family                                 |

> Response Example

```json
{
  "code": "0",
  "data": [
    {
      "frozenInterval": "2000",
      "instFamily": "ETH-USD",
      "mmpFrozen": true,
      "mmpFrozenUntil": "1000",
      "qtyLimit": "10",
      "timeInterval": "5000"
    }
  ],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                      |
| :--------- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------|
| instFamily     | String   | Instrument Family                                                                                                                    |
| mmpFrozen    | Boolean   | Whether MMP is currently triggered. `true` or `false`                                                                                |
| mmpFrozenUntil    | String   | If frozenInterval is configured and mmpFrozen = True, it is the time interval (in ms) when MMP is no longer triggered, otherwise "". |
| timeInterval      | String   | Time window (ms). MMP interval where monitoring is done                                                                              |
| frozenInterval      | String   | Frozen period (ms). If it is "0", the trade will remain frozen until manually reset and `mmpFrozenUntil` will be "".                   |
| qtyLimit      | String   | Trade qty limit in number of contracts                                                                                               |
