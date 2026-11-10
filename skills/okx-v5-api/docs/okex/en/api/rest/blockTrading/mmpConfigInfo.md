---
site: okex
lang: en
name: Get MMP Config
method: GET
path: /api/v5/rfq/mmp-config
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get MMP Config

This endpoint is used to get MMP configure information and only applicable to block trading market makers<br>

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/rfq/mmp-config`

> Request Example

```shell
GET /api/v5/rfq/mmp-config

```

#### Request Parameters
none

> Response Example

```json
{
  "code": "0",
  "data": [
    {
      "frozenInterval": "2000",
      "mmpFrozen": true,
      "mmpFrozenUntil": "1000",
      "countLimit": "10",
      "timeInterval": "5000"
    }
  ],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                         |
| :--------- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| timeInterval      | String   | Time window (ms). MMP interval where monitoring is done<br>"0" means MMP is diabled                                                                                                                         |
| frozenInterval      | String   | Frozen period (ms). If it is "0", the trade will remain frozen until manually reset and `mmpFrozenUntil` will be "".                                                                                                                                                                                                |
| countLimit      | String   | Limit in number of execution attempts                                                                                                                                                                                                |
| mmpFrozen    | Boolean   | Whether MMP is currently triggered. `true` or `false`                                                                                                                                                       |
| mmpFrozenUntil    | String   | If frozenInterval is not "0" and mmpFrozen = True, it is the time interval (in ms) when MMP is no longer triggered, otherwise ""                                                                        |
