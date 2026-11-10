---
site: us
lang: en
name: Set MMP
method: POST
path: /api/v5/account/mmp-config
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Set MMP

This endpoint is used to set MMP configure<br>

Only applicable to Option in Portfolio Margin mode, and MMP privilege is required.

<aside class="notice"><br>
What is MMP?<br>
Market Maker Protection (MMP) is an automated mechanism for market makers to pull their quotes when their executions exceed a certain threshold(`qtyLimit`) within a certain time frame(`timeInterval`). Once mmp is triggered, any pre-existing mmp pending orders(`mmp` and `mmp_and_post_only` orders) will be automatically canceled, and new orders tagged as MMP will be rejected for a specific duration(`frozenInterval`), or until manual reset by makers.<br>
<br>
How to enable MMP?<br>
Please send an email to institutional@okx.com or contact your business development (BD) manager to apply for MMP. The initial threshold will be upon your request.
</aside>

<aside class="notice">
MMP is configured individually per instrument family (<code>instFamily</code>). Enabling MMP for one instrument family does <b>not</b> automatically extend to others. For example, setting up MMP for <code>BTC-USD</code> does not cover <code>ETH-USD</code> or <code>SOL-USD</code> — each must be configured separately via this endpoint.
</aside>

#### Rate Limit: 2 requests per 10 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/mmp-config`

> Request Example

```shell
POST /api/v5/account/mmp-config
body
{
    "instFamily":"BTC-USD",
    "timeInterval":"5000",
    "frozenInterval":"2000",
    "qtyLimit": "100"
}

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                      |
| :-------- | :----- |:---------|:-----------------------------------------------------------------------------------------------------------------|
| instFamily  | String | Yes      | Instrument family                                                                                                |
| timeInterval  | String | Yes       | Time window (ms). MMP interval where monitoring is done<br>"0" means disable MMP                                |
| frozenInterval  | String | Yes       | Frozen period (ms). <br>"0" means the trade will remain frozen until you request "Reset MMP Status" to unfrozen |
| qtyLimit  | String | Yes       | Trade qty limit in number of contracts<br>Must be > 0                                                           |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "frozenInterval":"2000",
        "instFamily":"BTC-USD",
        "qtyLimit": "100",
        "timeInterval":"5000"
    }
  ]
}
```

#### Response Parameters

| Parameter | Type   | Description                           |
| :-------- | :----- |:--------------------------------------|
| instFamily     | String | Instrument family |
| timeInterval     | String | Time window (ms). MMP interval where monitoring is done |
| frozenInterval     | String | Frozen period (ms).|
| qtyLimit     | String | Trade qty limit in number of contracts |
