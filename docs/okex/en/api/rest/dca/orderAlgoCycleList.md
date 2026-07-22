---
site: okex
lang: en
name: GET / DCA cycle list
method: GET
path: /api/v5/tradingBot/dca/cycle-list
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / DCA cycle list
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`GET /api/v5/tradingBot/dca/cycle-list`

> Request Example

```shell
GET /api/v5/tradingBot/dca/cycle-list?algoId=2833925189933756416&algoOrdType=contract_dca
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                                          |
| :---------- | :----- | :------- | :----------------------------------------------------------------------------------- |
| algoId      | String | Yes      | Algo ID                                                                              |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order  |
| instId      | String | No       | Instrument ID                                                                        |
| after       | String | No       | Pagination of data to return records earlier than the requested `cycleId`            |
| before      | String | No       | Pagination of data to return records newer than the requested `cycleId`              |
| limit       | String | No       | Number of results per request. The maximum is 100. The default is 100                |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoClOrdId": "",
            "cycleId": "9876543",
            "currentCycle": true,
            "realizedPnl": "12.5",
            "startTime": "1597026383085",
            "endTime": "",
            "fee": "-0.3",
            "avgPx": "41500",
            "tpPx": "43000"
        }
    ]
}
```

#### Response Parameters
| Parameter    | Type    | Description                                                                                                                       |
| :----------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------- |
| algoId       | String  | Algo ID                                                                                                                           |
| algoClOrdId  | String  | Client-supplied Algo ID                                                                                                           |
| cycleId      | String  | Cycle ID                                                                                                                          |
| currentCycle | Boolean | Whether it is the current cycle<br>`true` or `false`                                                                             |
| realizedPnl  | String  | Realized PnL                                                                                                                      |
| startTime    | String  | Cycle start time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                    |
| endTime      | String  | Cycle end time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                      |
| fee          | String  | Accumulated fee in the cycle<br>Negative number represents the user transaction fee charged by the platform. Positive number represents rebate. |
| avgPx        | String  | Average open price                                                                                                                |
| tpPx         | String  | Take-profit price                                                                                                                 |
