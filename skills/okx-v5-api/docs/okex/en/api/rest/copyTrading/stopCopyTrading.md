---
site: okex
lang: en
name: POST / Stop copying
method: POST
path: /api/v5/copytrading/stop-copy-trading
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Stop copying

You need to use this endpoint to stop copy trading

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`POST /api/v5/copytrading/stop-copy-trading`

> Request example

```
POST /api/v5/copytrading/stop-copy-trading
body
{
    "instType": "SWAP",
    "uniqueCode": "25CD5A80241D6FE6",
    "subPosCloseType": "manual_close"
}
```

#### Request Parameters

| Parameter  | Type   | Required    | Description                                                                                                                                                                                                         |
| :------ | :----- |:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No          | Instrument type<br>`SWAP`                                                                                                                                                                                          |
| uniqueCode  | String | Yes         | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                                       |
| subPosCloseType     | String | Yes         | Action type for open positions, it is required if you have related copy position<br>`market_close`: immediately close at market price<br>`copy_close`：close when trader closes<br>`manual_close`: close manually |

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

| **Parameter**  | **Type** | **Description**                                    |
| :---------- | :------- |:---------------------------------------------------|
| result      | Boolean   | The result of setting <br>`true` |
