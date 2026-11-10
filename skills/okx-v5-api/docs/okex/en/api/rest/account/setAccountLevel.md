---
site: okex
lang: en
name: Set account mode
method: POST
path: /api/v5/account/set-account-level
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set account mode

You need to set on the Web/App for the first set of every account mode. If users plan to switch account modes while holding positions, they should first call the preset endpoint to conduct necessary settings, then call the precheck endpoint to get unmatched information, margin check, and other related information, and finally call the account mode switch endpoint to switch account modes.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-account-level`

> Request Example

```shell
POST /api/v5/account/set-account-level
body
{
    "acctLv":"1"
}
```

#### Request Parameters

| Parameter     | Type   | Required | Description                                                                                                                                 |
| :--------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------|
| acctLv  | String | Yes     | Account mode<br>`1`: Spot mode<br>`2`: Futures mode <br>`3`: Multi-currency margin code <br>`4`: Portfolio margin mode |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "1"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter     | Type   | Description                                                                                                                        |
| :--------- | :----- |:-----------------------------------------------------------------------------------------------------------------------------------|
| acctLv   | String | Account mode |
