---
site: okex
lang: en
name: POST / First copy settings
method: POST
path: /api/v5/copytrading/first-copy-settings
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / First copy settings

The first copy settings for the certain lead trader. You need to first copy settings after stopping copying.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`POST /api/v5/copytrading/first-copy-settings`

> Request example

```
POST /api/v5/copytrading/first-copy-settings
body
{
    "instType": "SWAP",
    "uniqueCode": "25CD5A80241D6FE6",
    "copyMgnMode": "cross",
    "copyInstIdType": "copy",
    "copyMode": "ratio_copy",
    "copyRatio": "1",
    "copyTotalAmt": "500",
    "subPosCloseType": "copy_close"
}
```

#### Request Parameters

| Parameter  | Type   | Required    | Description                                                                                                                                                                                                                                     |
| :------ | :----- |:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No          | Instrument type<br>`SWAP`, the default value                                                                                                                                                                                                   |
| uniqueCode  | String | Yes         | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                                                                   |
| copyMgnMode     | String | Yes         | Copy margin mode<br>`cross`: cross<br>`isolated`: isolated<br>`copy`: Use the same margin mode as lead trader when opening positions                                                                                                        |
| copyInstIdType     | String | Yes         | Copy contract type setted<br>`custom`: custom by `instId` which is required；<br>`copy`: Keep your contracts consistent with this trader by automatically adding or removing contracts when they do                                            |
| instId     | String | Conditional | Instrument ID. <br>If there are multiple instruments, separate them with commas.                                                                                                                    |
| copyMode     | String | No          | Copy mode<br>`fixed_amount`: set the same fixed amount for each order, and `copyAmt` is required；<br>`ratio_copy`: set amount as a multiple of the lead trader’s order value, and `copyRatio` is required <br>The default is `fixed_amount` |
| copyTotalAmt     | String | Yes         | Maximum total amount in USDT. <br>The maximum total amount you'll invest at any given time across all orders in this copy trade<br/>You won’t copy new orders if you exceed this amount                                                                                                                |
| copyAmt     | String | Conditional | Copy amount per order in USDT.                                                                                                                                                                                                                  |
| copyRatio     | String | Conditional | Copy ratio per order.                                                                                                                                                                                                                           |
| tpRatio     | String | No          | Take profit per order. 0.1 represents 10%                                                                                                                                                                                                       |
| slRatio     | String | No          | Stop loss per order. 0.1 represents 10%                                                                                                                                                                                                         |
| slTotalAmt     | String | No          | Total stop loss in USDT for trader. <br>If your net loss (total profit - total loss) reaches this amount, you'll stop copying this trader                                                                                                      |
| subPosCloseType     | String | Yes         | Action type for open positions<br>`market_close`: immediately close at market price<br>`copy_close`：close when trader closes<br>`manual_close`: close manually<br>The default is `copy_close`                                               |
| tag       | String | No      | Order tag<br/>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                            |

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
