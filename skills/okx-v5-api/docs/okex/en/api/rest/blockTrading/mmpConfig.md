---
site: okex
lang: en
name: Set MMP
method: POST
path: /api/v5/rfq/mmp-config
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set MMP

This endpoint is used to set MMP configure and only applicable to block trading makers<br>

#### Rate Limit: 1 request per 10 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/rfq/mmp-config`

> Request Example

```shell
POST /api/v5/rfq/mmp-config
body
{
    "timeInterval":"5000",
    "frozenInterval":"2000",
    "countLimit": "100"
}

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                      |
| :-------- | :----- |:---------|:-----------------------------------------------------------------------------------------------------------------|
| timeInterval  | String | Yes       | Time window (ms). MMP interval where monitoring is done.<br>"0" means disable MMP. Maximum time interval is 600,000.          |
| frozenInterval  | String | Yes       | Frozen period (ms). <br>"0" means the trade will remain frozen until you request "Reset MMP Status" to unfrozen. |
| countLimit  | String | Yes      | Limit in number of execution attempts.                                                                |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "frozenInterval":"2000",
        "countLimit": "100",
        "timeInterval":"5000"
    }
  ]
}
```

#### Response Parameters

| Parameter | Type   | Description                           |
| :-------- | :----- |:--------------------------------------|
| timeInterval     | String | Time window (ms). MMP interval where monitoring is done |
| frozenInterval     | String | Frozen period (ms).|
| countLimit     | String | Limit in number of execution attempts |

<aside class="notice">
Group RFQ introduction<br><br>

For RFQ makers, the execution attempt of group RFQ will only count once towards MMP regardless of how many account allocations involved.
</aside>
