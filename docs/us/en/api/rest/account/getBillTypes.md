---
site: us
lang: en
name: Get bill types
method: GET
path: /api/v5/account/subtypes
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Get bill types

Get all bill types, and the mapping of bill type and subType.

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: UserId

#### HTTP request

`GET /api/v5/account/subtypes`

> Request example

```shell
GET /api/v5/account/subtypes
```

#### Request parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| type | String | No | Bill type. Multiple values are supported, separated by commas, e.g. `1,2,3`. If not specified, all types are returned. |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "type": "1",
            "typeDesc": "Transfer",
            "subTypeDetails": [
                {
                    "subType": "11",
                    "subTypeDesc": "Transfer in"
                },
                {
                    "subType": "12",
                    "subTypeDesc": "Transfer out"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| type | String | Bill type |
| typeDesc | String | Bill type description, "" means the type is not enabled. |
| subTypeDetails | Array of objects | Sub-type details |
| > subType | String | Sub-type |
| > subTypeDesc | String | Sub-type description, "" means the type is not enabled. |
