---
site: hongkong
lang: en
name: GET / Transaction details (in the past 2 years)
method: GET
path: /api/v5/trade/fills-archive
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Transaction details (in the past 2 years)

Retrieve recently-filled transaction details in the past 2 years except for last 3 months.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/trade/fills-archive`

> Request Example

```shell
GET /api/v5/trade/fills-archive?year=2023&quarter=Q4

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- | :------- | :----------------------------------------------------------- |
| year   | String | Yes       | 4 digits year  |
| quarter| String | Yes       | Quarter, valid value is `Q1`, `Q2`, `Q3`, `Q4`      |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```
#### Response Parameters

| **Parameter** | **Type** | **Description**                     |
| :------------ | :------- | :--------------------------------- |
| fileHref   | String | Download file link                                                    |
| ts    | String | Download link generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`              |
| state   | String | Download link status <br>"finished" "ongoing"                |

#### Field descriptions in the decompressed CSV file

| **Parameter** | **Type** | **Description**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType   | String   | Instrument type                                                     |
| instId     | String   | Instrument ID                                                   |
| tradeId    | String   | Last trade ID                                                   |
| ordId      | String   | Order ID                                                       |
| clOrdId    | String   | Client Order ID as assigned by the client                      |
| billId     | String   | Bill ID                                                       |
| tag        | String   | Order tag                                                     |
| fillPx     | String   | Last filled price                                                 |
| fillSz     | String   | Last filled quantity                                                 |
| side       | String   | Order side,  `buy`  `sell`                              |
| posSide    | String   | Position side <br>`long`  `short`   <br>it returns `net` in`net` mode. |
| execType   | String   | Liquidity taker or maker<br>`T`: taker<br>`M`: maker<br>Not applicable to system orders such as ADL and liquidation |
| feeCcy     | String   | Trading fee or rebate currency                                               |
| fee        | String   | The amount of trading fee or rebate. The trading fee deduction is negative, such as '-0.01'; the rebate is positive, such as '0.01'.                                               |
| ts         | String   | Data generation time,  Unix timestamp format in milliseconds, e.g. `1597026383085`. |
| subType    | String   | Transaction type |
