---
site: okex
lang: en
name: GET / Lead trader lead position history
method: GET
path: /api/v5/copytrading/public-subpositions-history
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Lead trader lead position history

Public endpoint. Retrieve the lead trader completed leading position of the last 3 months.<br>
Returns reverse chronological order with `subPosId`. 

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-subpositions-history`

> Request example

```shell
GET /api/v5/copytrading/public-subpositions-history?instType=SWAP&uniqueCode=9A8534AB09862774

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                              |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value.                                                                                                                                  |
| uniqueCode  | String | Yes      | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters)                                                |
| after    | String | No       | Pagination of data to return records earlier than the requested `subPosId`.        |
| before   | String | No       | Pagination of data to return records newer than the requested `subPosId`.                                         |
| limit    | String | No       | Number of results per request. Maximum is 100. Default is 100.                                                    |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "closeAvgPx": "28385.9",
            "closeTime": "1697709137162",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "20",
            "margin": "4.245285",
            "mgnMode": "isolated",
            "openAvgPx": "28301.9",
            "openTime": "1697698048031",
            "pnl": "0.252",
            "pnlRatio": "0.05935997229868",
            "posSide": "long",
            "subPos": "3",
            "subPosId": "635126416883355648",
            "uniqueCode": "9A8534AB09862774"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                       |
| :---------- | :------- |:----------------------------------------------------------------------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                                                     |
| subPosId      | String   | Lead position ID                                                                                                      |
| posSide         | String   | Position side<br>`long` <br>`short` <br>`net`<br>(long position has positive subPos; short position has negative subPos) |
| mgnMode       | String   | Margin mode. `cross` `isolated`                                                                                       |
| lever     | String   | Leverage                                                                                                              |
| openAvgPx          | String   | Average open price                                                                                                    |
| openTime          | String   | Time of opening                                                                                                       |
| subPos         | String   | Quantity of positions                                                                                                 |
| closeTime     | String   | Time of closing position                                                                                              |
| closeAvgPx        | String   | Average price of closing position                                                                                     |
| pnl     | String   | Profit and loss                                                                                                       |
| pnlRatio     | String   | P&L ratio                                                                                                             |
| instType  | String     | Instrument type                                                                                                       |
| margin     | String     | Margin                                                                                                                |
| ccy     | String       | Currency                                                                                                              |
| uniqueCode     | String       | Lead trader unique code                                                                                               |
