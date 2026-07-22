---
site: okex
lang: en
name: Set auto earn
method: POST
path: /api/v5/account/set-auto-earn
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Set auto earn

Turn on/off auto earn.

#### Rate limit: 2 requests per 2 seconds

#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-auto-earn`

> Request example

```shell
// turn on auto lend
{
   "earnType": "0",
   "ccy":"BTC",
   "action":"turn_on"
}

```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| earnType | String | No | Auto earn type<br>`0`: auto earn (auto lend, auto staking) <br>`1`: auto earn (USDG earn)<br> The default value is `0` |
| ccy | String | Yes | Currency |
| action | String | Yes | Auto earn operation action<br>`turn_on`: turn on auto earn<br>`turn_off`: turn off auto earn<br>~~`amend`: amend minimum lending APR, applicable only to earnType `0`~~  (Deprecated) |
| apr | String | Optional | ~~Minimum lending APR. Users must pass in this field when earnType is `0` and action is `turn_on/amend`.<br> 0.01 means 1%, available range 0.01-3.65, increment 0.01~~  (Deprecated) |

> Response example

```json
{
   "code":"0",
   "msg":"",
   "data":[
      {
         "earnType": "0",
         "ccy":"BTC",
         "action":"turn_on",
         "apr":"0.01"
      }
   ]
}

```

#### Response parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| earnType | String | Auto earn type<br>`0`: auto earn (auto lend, auto staking) <br>`1`: auto earn (USDG earn) |
| ccy | String | Currency |
| action | String | Auto earn operation action<br> `turn_on`<br> `turn_off`<br> ~~`amend`~~  (Deprecated) |
| apr | String | ~~Minimum lending APR~~  (Deprecated) |
