---
site: okex
lang: en
name: GET / DCA sub orders
method: GET
path: /api/v5/tradingBot/dca/orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / DCA sub orders
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID
#### HTTP Request
`GET /api/v5/tradingBot/dca/orders`

> Request Example

```shell
GET /api/v5/tradingBot/dca/orders?algoId=2833925189933756416&algoOrdType=contract_dca
```

#### Request Parameters
| Parameter   | Type   | Required | Description                                                                          |
| :---------- | :----- | :------- | :----------------------------------------------------------------------------------- |
| algoId      | String | Yes      | Algo ID                                                                              |
| algoOrdType | String | Yes      | Algo order type<br>`contract_dca`: Contract DCA order<br>`spot_dca`: Spot DCA order  |
| cycleId     | String | No       | Cycle ID                                                                             |
| after       | String | No       | Pagination of data to return records earlier than the requested `ordId`              |
| before      | String | No       | Pagination of data to return records newer than the requested `ordId`                |
| limit       | String | No       | Number of results per request. The maximum is 100. The default is 100                |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "cycleId": "9876543",
            "ordId": "570627699870375936",
            "avgFillPx": "41500",
            "direction": "long",
            "side": "buy",
            "ordType": "init_order",
            "px": "41000",
            "sz": "10",
            "filledSz": "10",
            "state": "filled",
            "fee": "-0.2",
            "rebate": "0",
            "rebateCcy": "USDT",
            "lever": "3",
            "instId": "BTC-USDT-SWAP",
            "ctVal": "0.01",
            "fillTime": "1597026383085",
            "cTime": "1597026383085",
            "uTime": "1597026383085",
            "tradeQuoteCcy": ""
        }
    ]
}
```

#### Response Parameters
| Parameter     | Type   | Description                                                                                                                                                                                                       |
| :------------ | :----- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cycleId       | String | Cycle ID                                                                                                                                                                                                          |
| ordId         | String | Sub order ID                                                                                                                                                                                                      |
| avgFillPx     | String | Average filled price                                                                                                                                                                                              |
| direction     | String | Position direction<br>Contract DCA: `long`: Long position, `short`: Short position<br>Spot DCA: `long`: Long position                                                                                             |
| side          | String | Order side<br>`buy`<br>`sell`                                                                                                                                                                                     |
| ordType       | String | Sub order type<br>`init_order`: Initial order<br>`safety_order`: Safety order<br>`tp_order`: Take-profit order<br>`sl_order`: Stop-loss order<br>`manual_add_order`: Manually added order<br>`close_position`: Close position order<br>`manual_close_position`: Manual close position order |
| px            | String | Order price                                                                                                                                                                                                       |
| sz            | String | Order size                                                                                                                                                                                                        |
| filledSz      | String | Filled size                                                                                                                                                                                                       |
| state         | String | Order status<br>`live`: Pending fill<br>`partially_filled`: Partially filled<br>`filled`: Fully filled<br>`canceled`: Canceled<br>`cancelling`: Cancelling                                                        |
| fee           | String | Accumulated fee<br>Negative number represents the user transaction fee charged by the platform. Positive number represents rebate.                                                                                |
| rebate        | String | Rebate amount                                                                                                                                                                                                     |
| rebateCcy     | String | Rebate currency                                                                                                                                                                                                   |
| lever         | String | Leverage<br>Only applicable to `contract_dca`                                                                                                                                                                     |
| instId        | String | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                              |
| ctVal         | String | Contract value<br>Only applicable to `contract_dca`                                                                                                                                                               |
| fillTime      | String | Last filled time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                     |
| cTime         | String | Order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                   |
| uTime         | String | Order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                   |
| tradeQuoteCcy | String | Quote currency for trading<br>Only applicable to `spot_dca`                                                                                                                                                      |
