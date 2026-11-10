---
site: okex
lang: en
name: GET / Copy settings
method: GET
path: /api/v5/copytrading/copy-settings
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Copy settings

Retrieve the copy settings about certain lead trader.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/copy-settings`

> Request example

```shell
GET /api/v5/copytrading/copy-settings?instType=SWAP&uniqueCode=25CD5A80241D6FE6

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                     |
| :------ | :----- |:-----|:--------------------------------------------------------------------------------|
| instType     | String | No          | Instrument type<br>`SWAP`                                                                                                                                                                                          |
| uniqueCode  | String | Yes         | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                                       |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "copyAmt": "",
            "copyInstIdType": "copy",
            "copyMgnMode": "isolated",
            "copyMode": "ratio_copy",
            "copyRatio": "1",
            "copyState": "1",
            "copyTotalAmt": "500",
            "instIds": [
                {
                    "enabled": "1",
                    "instId": "ADA-USDT-SWAP"
                },
                {
                    "enabled": "1",
                    "instId": "YFII-USDT-SWAP"
                }
            ],
            "slRatio": "",
            "slTotalAmt": "",
            "subPosCloseType": "copy_close",
            "tpRatio": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                    |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| copyMode       | String   | Copy mode<br>`fixed_amount` `ratio_copy`                                                                                                                                                           |
| copyAmt        | String   | Copy amount in USDT per order.                                                                                                                                                                     |
| copyRatio      | String   | Copy ratio per order.                                                                                                                                                                              |
| copyTotalAmt      | String   | Maximum total amount in USDT. <br>The maximum total amount you'll invest at any given time across all orders in this copy trade                                                                                                                                                                              |
| tpRatio        | String   | Take profit per order. 0.1 represents 10%                                                                                                                                                          |
| slRatio        | String   | Stop loss per order. 0.1 represents 10%                                                                                                                                                            |
| copyInstIdType | String   | Copy contract type setted<br>`custom`: custom by `instId` which is required；<br>`copy`: Keep your contracts consistent with this trader by automatically adding or removing contracts when they do |
| instIds        | Array of objects   | Instrument list. It will return all lead contracts of the lead trader                                                                                                                              |
| > instId             | String   | Instrument ID                                                                                                                                                                                      |
| > enabled         | String   | Whether copying this `instId`<br>`0` `1`                                                                                                                                                           |
| slTotalAmt      | String   | Total stop loss in USDT for trader.                                                                                                                                                                |
| subPosCloseType     | String   | Action type for open positions<br>`market_close`: immediately close at market price<br>`copy_close`：close when trader closes<br>`manual_close`: close manually                                     |
| copyMgnMode            | String   | Copy margin mode<br>`cross`: cross<br>`isolated`: isolated<br>`copy`: Use the same margin mode as lead trader when opening positions                                                               |
| ccy       | String   | Margin currency                                                                                                                                                                                    |
| copyState       | String     | Current copy state <br>`0`: non-copy, `1`: copy                                                                                                                                                    |
| tag         | String | Order tag                                                                                                                                                                                                |
