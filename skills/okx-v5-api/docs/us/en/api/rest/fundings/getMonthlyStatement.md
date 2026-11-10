---
site: us
lang: en
name: Get monthly statement (last year)
method: GET
path: /api/v5/asset/monthly-statement
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get monthly statement (last year)

Retrieve monthly statement in the past year.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/monthly-statement`

> Request Example

```shell
GET /api/v5/asset/monthly-statement?month=Jan

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- | :------- | :----------------------------------------------------------- |
| month | String | Yes       | Month, valid value is `Jan`, `Feb`, `Mar`, `Apr`,`May`, `Jun`, `Jul`,`Aug`, `Sep`,`Oct`,`Nov`,`Dec`     |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": 1646892328000
        }
    ],
    "msg": ""
}
```
#### Response Parameters

| **Parameter** | **Type** | **Description**                     |
| :------------ |:---------| :--------------------------------- |
| fileHref   | String   | Download file link                                                    |
| ts    | Int      | Download link generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`              |
| state   | String   | Download link status <br>"finished" "ongoing"                |
