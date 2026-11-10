---
site: okex
lang: en
name: GET / Existing lead positions
method: GET
path: /api/v5/copytrading/current-subpositions
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Existing lead positions

Retrieve lead positions that are not closed.<br>

Returns reverse chronological order with `openTime`

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/current-subpositions`

> Request example

```shell
GET /api/v5/copytrading/current-subpositions?instId=BTC-USDT-SWAP

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                       |
| :------ | :----- |:-----|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`<br>It returns all types by default.                                                                                           |
| instId  | String | No    | Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                 |
| after    | String | No       | Pagination of data to return records earlier than the requested `subPosId`.                                                                                       |
| before   | String | No       | Pagination of data to return records newer than the requested `subPosId`.                                                                                         |
| limit    | String | No       | Number of results per request. Maximum is 500. Default is 500.                                                                                                    |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "",
            "ccy": "USDT",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "12.6417",
            "markPx": "38205.8",
            "mgnMode": "isolated",
            "openAvgPx": "37925.1",
            "openOrdId": "",
            "openTime": "1701231120479",
            "posSide": "net",
            "slOrdPx": "",
            "slTriggerPx": "",
            "subPos": "1",
            "subPosId": "649945658862370816",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "uniqueCode": "25CD5A80241D6FE6",
            "upl": "0.2807",
            "uplRatio": "0.0222042921442527",
            "availSubPos": "1"
        },
        {
            "algoId": "",
            "ccy": "USDT",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "12.6263333333333333",
            "markPx": "38205.8",
            "mgnMode": "isolated",
            "openAvgPx": "37879",
            "openOrdId": "",
            "openTime": "1701225074786",
            "posSide": "net",
            "slOrdPx": "",
            "slTriggerPx": "",
            "subPos": "1",
            "subPosId": "649920301388038144",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "uniqueCode": "25CD5A80241D6FE6",
            "upl": "0.3268",
            "uplRatio": "0.0258824150584758",
            "availSubPos": "1"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**  | **Type** | **Description**                                                                                                                 |
| :---------- | :------- |:--------------------------------------------------------------------------------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                                                               |
| subPosId      | String   | Lead position ID                                                                                                                |
| posSide         | String   | Position side<br>`long` <br>`short` <br>`net`<br>(Long positions have positive subPos; short positions have negative subPos)    |
| mgnMode       | String   | Margin mode. `cross` `isolated`                                                                                                 |
| lever     | String   | Leverage                                                                                                                        |
| openOrdId         | String   | Order ID for opening position, only applicable to lead position                                                                 |
| openAvgPx          | String   | Average open price                                                                                                              |
| openTime          | String   | Open time                                                                                                                       |
| subPos         | String   | Quantity of positions                                                                                                           |
| tpTriggerPx     | String   | Take-profit trigger price.                                                                                                      |
| slTriggerPx        | String   | Stop-loss trigger price.                                                                                                        |
| algoId     | String   | Stop order ID                                                                                                                   |
| instType  | String     | Instrument type                                                                                                                 |
| tpOrdPx     | String    | Take-profit order price, it is -1 for market price                                                                              |
| slOrdPx     | String    | Stop-loss order price, it is -1 for market price                                                                                |
| margin     | String     | Margin                                                                                                                          |
| upl     | String     | Unrealized profit and loss                                                                                                      |
| uplRatio     | String      | Unrealized profit and loss ratio                                                                                                |
| markPx     | String       | Latest mark price, only applicable to contract                                                                                  |
| uniqueCode     | String       | Lead trader unique code                                                                                                         |
| ccy     | String       | Margin currency                                                                                                                 |
| availSubPos     | String    | Quantity of positions that can be closed                                                                                        |
