---
site: okex
lang: en
name: GET / My lead traders
method: GET
path: /api/v5/copytrading/current-lead-traders
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / My lead traders

Retrieve my lead traders.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/current-lead-traders`

> Request example

```shell
GET /api/v5/copytrading/current-lead-traders?instType=SWAP

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                        |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No          | Instrument type<br>`SWAP`, the default value                                                                                                                                                                                                   |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "beginCopyTime": "1701224821936",
            "ccy": "USDT",
            "copyTotalAmt": "500",
            "copyTotalPnl": "0",
            "leadMode": "public",
            "margin": "1.89395",
            "nickName": "Trader9527",
            "portLink": "",
            "profitSharingRatio": "0.08",
            "todayPnl": "0",
            "uniqueCode": "25CD5A80241D6FE6",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                              |
| :---------- | :------- |:-----------------------------------------------------------------------------|
| portLink           | String   | Portrait link                                                                |
| nickName          | String   | Nick name                                                                    |
| margin | String   | Margin for copy trading                                                      |
| copyTotalAmt | String   | Copy total amount                                                            |
| copyTotalPnl | String   | Copy total pnl                                                               |
| uniqueCode | String   | Lead trader unique code                                                      |
| ccy | String   | margin currency                                                              |
| profitSharingRatio | String   | Profit sharing ratio. 0.1 represents 10%                                     |
| beginCopyTime | String   | Begin copying time. Unix timestamp format in milliseconds, e.g.1597026383085 |
| upl | String   | Unrealized profit & loss                                                     |
| todayPnl | String   | Today pnl                                                                    |
| leadMode | String   | Lead mode `public` `private`                                                 |
