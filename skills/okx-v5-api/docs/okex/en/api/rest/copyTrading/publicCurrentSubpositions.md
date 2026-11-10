---
site: okex
lang: en
name: GET / Lead trader current lead positions
method: GET
path: /api/v5/copytrading/public-current-subpositions
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Lead trader current lead positions

Public endpoint. Get current leading positions of lead trader

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-current-subpositions`

> Request example

```shell
GET /api/v5/copytrading/public-current-subpositions?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                   |
| :------ | :----- |:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value.                                                                                                                                 |
| uniqueCode     | String | Yes      | Lead trader unique code<br>A combination of case-sensitive alphanumerics, all numbers and the length is 16 or 18 characters, e.g. 213E8C92DC61EFAC (16 characters) or 381749205163847291 (18 characters) |
| after    | String | No       | Pagination of data to return records earlier than the requested `subPosId`.                                                                                                   |
| before   | String | No       | Pagination of data to return records newer than the requested `subPosId`.                                                                                                     |
| limit    | String | No       | Number of results per request. Maximum is 100. Default is 100.                                                                                                                |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "lever": "5",
            "margin": "16.23304",
            "markPx": "2027.31",
            "mgnMode": "isolated",
            "openAvgPx": "2029.13",
            "openTime": "1701144639417",
            "posSide": "short",
            "subPos": "4",
            "subPosId": "649582930998104064",
            "uniqueCode": "D9ADEAB33AE9EABD",
            "upl": "0.0728",
            "uplRatio": "0.0044846806266725"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**  | **Type** | **Description**                                                                                                               |
| :---------- | :------- |:------------------------------------------------------------------------------------------------------------------------------|
| instId      | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                                                             |
| subPosId      | String   | Lead position ID                                                                                                              |
| posSide         | String   | Position side<br>`long` <br>`short` <br>`net`<br>(Long positions have positive subPos; short positions have negative subPos) |
| mgnMode       | String   | Margin mode. `cross` `isolated`                                                                                               |
| lever     | String   | Leverage                                                                                                                      |
| openAvgPx          | String   | Average open price                                                                                                            |
| openTime          | String   | Open time                                                                                                                     |
| subPos         | String   | Quantity of positions                                                                                                         |
| instType  | String     | Instrument type                                                                                                               |
| margin     | String     | Margin                                                                                                                        |
| upl     | String     | Unrealized profit and loss                                                                                                    |
| uplRatio     | String      | Unrealized profit and loss ratio                                                                                              |
| markPx     | String       | Latest mark price, only applicable to contract                                                                                |
| uniqueCode     | String       | Lead trader unique code                                                                                                       |
| ccy     | String       | Currency                                                                                                                      |
