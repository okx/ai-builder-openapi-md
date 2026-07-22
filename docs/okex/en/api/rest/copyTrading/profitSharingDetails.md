---
site: okex
lang: en
name: GET / Profit sharing details
method: GET
path: /api/v5/copytrading/profit-sharing-details
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Profit sharing details

The leading trader gets profits shared details for the last 3 months.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/profit-sharing-details`

> Request example

```shell
GET /api/v5/copytrading/profit-sharing-details?limit=2

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`<br>It returns all types by default.   |
| after    | String | No       | Pagination of data to return records earlier than the requested `profitSharingId` |
| before   | String | No       | Pagination of data to return records newer than the requested `profitSharingId` |
| limit    | String | No       | Number of results per request. Maximum is 100. Default is 100.         |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "nickName": "Potato",
            "profitSharingAmt": "0.00536",
            "profitSharingId": "148",
            "portLink": "",
            "ts": "1723392000000",
            "instType": "SWAP"
        },
        {
            "ccy": "USDT",
            "nickName": "Apple",
            "profitSharingAmt": "0.00336",
            "profitSharingId": "20",
            "portLink": "",
            "ts": "1723392000000",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                         |
| :---------- | :------- |:------------------------------------------------------------------------|
| ccy      | String   | The currency of profit sharing.                                         |
| profitSharingAmt      | String   | Profit sharing amount. It would be 0 if there is no any profit sharing. |
| nickName         | String   | Nickname of copy trader.                                                |
| profitSharingId       | String   | Profit sharing ID.                                                      |
| instType  | String     | Instrument type                                                         |
| portLink     | String    | Portrait link                                                           |
| ts     | String   | Profit sharing time.                                                    |
