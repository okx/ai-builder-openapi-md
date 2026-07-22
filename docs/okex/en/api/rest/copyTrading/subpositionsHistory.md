---
site: okex
lang: en
name: GET / Lead position history
method: GET
path: /api/v5/copytrading/subpositions-history
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Lead position history

Retrieve the completed lead position of the last 3 months.<br>
Returns reverse chronological order with `subPosId`. 

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/subpositions-history`

> Request example

```shell
GET /api/v5/copytrading/subpositions-history?instId=BTC-USDT-SWAP

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                     |
| :------ | :----- |:-----|:--------------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`<br>It returns all types by default.   |
| instId  | String | No    | Instrument ID, e.g. BTC-USDT-SWAP                                               |
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
            "closeAvgPx": "37617.5",
            "closeTime": "1701188587950",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "37.41",
            "markPx": "38203.4",
            "mgnMode": "isolated",
            "openAvgPx": "37410",
            "openOrdId": "",
            "openTime": "1701184638702",
            "pnl": "0.6225",
            "pnlRatio": "0.0166399358460306",
            "posSide": "net",
            "profitSharingAmt": "0.0407967",
            "subPos": "3",
            "closeSubPos": "2",
            "type": "1",
            "subPosId": "649750700213698561",
            "uniqueCode": "25CD5A80241D6FE6"
        },
        {
            "ccy": "USDT",
            "closeAvgPx": "37617.5",
            "closeTime": "1701188587950",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "24.94",
            "markPx": "38203.4",
            "mgnMode": "isolated",
            "openAvgPx": "37410",
            "openOrdId": "",
            "openTime": "1701184635381",
            "pnl": "0.415",
            "pnlRatio": "0.0166399358460306",
            "posSide": "net",
            "profitSharingAmt": "0.0271978",
            "subPos": "2",
            "closeSubPos": "2",
            "type": "2",
            "subPosId": "649750686292803585",
            "uniqueCode": "25CD5A80241D6FE6"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                          |
| :---------- | :------- |:-------------------------------------------------------------------------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                                                        |
| subPosId      | String   | Lead position ID                                                                                                         |
| posSide         | String   | Position side<br>`long` <br>`short` <br>`net`<br>(long position has positive subPos; short position has negative subPos) |
| mgnMode       | String   | Margin mode. `cross` `isolated`                                                                                          |
| lever     | String   | Leverage                                                                                                                 |
| openOrdId         | String   | Order ID for opening position, only applicable to lead position                                                          |
| openAvgPx          | String   | Average open price                                                                                                       |
| openTime          | String   | Time of opening                                                                                                          |
| subPos         | String   | Quantity of positions                                                                                                    |
| closeTime     | String   | Time of closing position                                                                                                 |
| closeAvgPx        | String   | Average price of closing position                                                                                        |
| pnl     | String   | Profit and loss                                                                                                          |
| pnlRatio     | String   | P&L ratio                                                                                                                |
| instType  | String     | Instrument type                                                                                                          |
| margin     | String     | Margin                                                                                                                   |
| ccy     | String       | Currency                                                                                                                 |
| markPx     | String       | Latest mark price, only applicable to contract                                                                           |
| uniqueCode     | String       | Lead trader unique code                                                                                                  |
| profitSharingAmt     | String       | Profit sharing amount, only applicable to copy trading. Note: this parameter is already deprecated.                      |
| closeSubPos     | String    | Quantity of positions that is already closed                                                                             |
| type     | String    | The type of closing position<br>`1`：Close position partially;<br>`2`：Close all                                           |
