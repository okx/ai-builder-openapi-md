---
site: okex
lang: en
name: Set settle currency
method: POST
path: /api/v5/account/set-settle-currency
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Set settle currency

Only applicable to USD-margined contract.

#### Rate limit: 20 times per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-settle-currency`

> Request Example

```shell
POST /api/v5/account/set-settle-currency
body
{
    "settleCcy": "USDC"
}

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                     |
| :------ | :----- | :------- |:-----------------------------------------------------------------------|
| settleCcy | String | Yes      | USD-margined contract settle currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data" :[
          {
            "settleCcy":"USDC"
          }
    ]  
}
```

#### Response Parameters

| Parameter  | Type   | Description   |
| :------ | :----- |:-----|
| settleCcy   | String | USD-margined contract settle currency |
