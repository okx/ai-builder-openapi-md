---
site: okex
lang: en
name: Get / DCA algo order position details
method: GET
path: /api/v5/tradingBot/dca/position-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get / DCA algo order position details
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`GET /api/v5/tradingBot/dca/position-details`

> Request Example

```shell
GET /api/v5/tradingBot/dca/position-details?algoId=2833925189933756416&algoOrdType=contract_dca
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                                              |
| :---------- | :----- | :------- | :--------------------------------------------------------------------------------------- |
| algoId      | String | Yes      | Algo order ID                                                                            |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order      |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoClOrdId": "",
            "algoOrdType": "contract_dca",
            "instId": "BTC-USDT-SWAP",
            "curCycleld": "3",
            "startTime": "1597026383085",
            "fillManualOrds": "0",
            "fillSafetyOrds": "2",
            "fundingFee": "-0.05",
            "initPx": "43200",
            "notionalUsd": "5000",
            "avgPx": "43000",
            "upl": "12.5",
            "liqPx": "38000",
            "sz": "2",
            "baseSz": "",
            "quoteSz": "",
            "slPx": "40000",
            "tpPx": "45000",
            "fee": "-0.2",
            "tradeQuoteCcy": ""
        }
    ]
}
```

#### Response Parameters
| Parameter      | Type   | Description                                                                                                                   |
| :------------- | :----- | :---------------------------------------------------------------------------------------------------------------------------- |
| algoId         | String | Algo order ID                                                                                                                 |
| algoClOrdId    | String | Client-supplied algo order ID                                                                                                 |
| algoOrdType    | String | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order                                          |
| instId         | String | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                          |
| curCycleld     | String | The cycle ID for the current cycle                                                                                            |
| startTime      | String | Start time of the current cycle, Unix timestamp in milliseconds, e.g. `1597026383085`                                        |
| fillManualOrds | String | Number of filled manually added orders in the current cycle                                                                   |
| fillSafetyOrds | String | Number of filled safety orders in the current cycle                                                                           |
| fundingFee     | String | Accumulated funding fee for the current cycle<br>Only applicable to `contract_dca`                                           |
| initPx         | String | Initial order average open price                                                                                              |
| notionalUsd    | String | Notional value of positions in USD<br>Only applicable to `contract_dca`                                                      |
| avgPx          | String | Average open price                                                                                                            |
| upl            | String | Unrealized PnL                                                                                                                |
| liqPx          | String | Estimated liquidation price<br>Only applicable to `contract_dca`                                                             |
| sz             | String | Position size in number of contracts<br>Only applicable to `contract_dca`                                                    |
| baseSz         | String | Amount of base currency held in the current cycle<br>Only applicable to `spot_dca`                                           |
| quoteSz        | String | Amount of quote currency held in the current cycle<br>Only applicable to `spot_dca`                                          |
| slPx           | String | Stop-loss price                                                                                                               |
| tpPx           | String | Take-profit price                                                                                                             |
| fee            | String | Accumulated fee. Negative number represents the transaction fee charged by the platform. Positive number represents rebate.  |
| tradeQuoteCcy  | String | The quote currency for trading<br>Only applicable to `spot_dca`                                                              |
