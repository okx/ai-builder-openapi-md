---
site: okex
lang: en
name: POST / Amend profit sharing ratio
method: POST
path: /api/v5/copytrading/amend-profit-sharing-ratio
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Amend profit sharing ratio

It is used to amend profit sharing ratio. 

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`POST /api/v5/copytrading/amend-profit-sharing-ratio`

> Request example

```
POST /api/v5/copytrading/amend-profit-sharing-ratio
body
{
    "instType": "SWAP",
    "profitSharingRatio": "0.1"
}

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                        |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`                                                                                                                                                                                         |
| profitSharingRatio     | String | Yes      | Profit sharing ratio. <br>0.1 represents 10%                                                                                                                                                                           |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "result": true
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                      |
| :---------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| result      | Boolean   | The result of setting <br>`true` |
