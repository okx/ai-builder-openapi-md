---
site: okex
lang: en
name: Set trading config
method: POST
path: /api/v5/account/set-trading-config
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Set trading config

#### Rate limit: 1 request per 2 seconds

#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-trading-config`

> Request example

```shell
POST /api/v5/account/set-trading-config
body
{
    "type": "stgyType",
    "stgyType":"1"
}

```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| type | String | Yes | Trading config type<br>`stgyType` |
| stgyType | String | No | Strategy type<br>`0`: general strategy<br>`1`: delta neutral strategy<br> Only applicable when type is `stgyType` |

> Response example

```json
{
   "code":"0",
   "msg":"",
   "data":[
      {
            "type": "stgyType",
            "stgyType":"1"
      }
   ]
}

```

#### Response parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| type | String | Trading config type |
| stgyType | String | Strategy type |
