---
site: okex
lang: en
name: GET / Signal bot order details
method: GET
path: /api/v5/tradingBot/signal/orders-algo-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Signal bot order details

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/orders-algo-details`

> Request Example

```shell
GET /api/v5/tradingBot/signal/orders-algo-details?algoId=623833708424069120&algoOrdType=contract
```

#### Request Parameters

| Parameter   | Type    | Required | Description                                                              |
|:------------| :------ | :------- |:-------------------------------------------------------------------------|
| algoOrdType | String | Yes       | Algo order type<br>`contract`: Contract signal |
| algoId      | String | Yes       | Algo ID                                                                  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "623833708424069120",
            "algoClOrdId": "",
            "algoOrdType": "contract",
            "availBal": "1.6561369013122267",
            "cTime": "1695005546360",
            "cancelType": "0",
            "entrySettingParam": {
                "allowMultipleEntry": true,
                "amt": "0",
                "entryType": "1",
                "ratio": ""
            },
            "exitSettingParam": {
                "slPct": "",
                "tpPct": "",
                "tpSlType": "price"
            },
            "floatPnl": "0.1279999999999927",
            "frozenBal": "25.16816",
            "instIds": [
                "BTC-USDT-SWAP",
                "ETH-USDT-SWAP"
            ],
            "instType": "SWAP",
            "investAmt": "100",
            "lever": "10",
            "ratio": "",
            "realizedPnl": "-73.303703098687766",
            "signalChanId": "623827579484770304",
            "signalChanName": "testing",
            "signalSourceType": "1",
            "state": "running",
            "subOrdType": "9",
            "totalEq": "26.824296901312227",
            "totalPnl": "-73.1757030986877733",
            "totalPnlRatio": "-0.7317570309868777",
            "uTime": "1697029422313"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**       | **Type** | **Description**                                                                                                                                                                                                        |
|:--------------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId               | String          | Algo ID                                                                                                                                                                                                                |
| algoClOrdId          | String          | Client-supplied Algo ID                                                                                                                                                                                                |
| instType             | String          | Instrument type                                                                                                                                                                                                        |
| instIds              | Array of strings | Instrument IDs                                                                                                                                                                                                         |
| cTime                | String          | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                   |
| uTime                | String          | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                   |
| algoOrdType          | String          | Algo order type<br>`contract`: Contract signal                                                                                                                                                                         |
| state                | String          | Algo order state<br>`starting`<br>`running`<br>`stopping`<br>`stopped`                                                                                                                                                 |
| cancelType           | String          | Algo order stop reason<br>`0`: None<br>`1`: Manual stop                                                                                                                                                                |
| totalPnl             | String          | Total P&L                                                                                                                                                                                                              |
| totalPnlRatio        | String          | Total P&L ratio                                                                                                                                                                                                              |
| totalEq              | String          | Total equity of strategy account                                                                                                                                                                                       |
| floatPnl             | String          | Float P&L                                                                                                                                                                                                              |
| realizedPnl          | String          | Realized P&L                                                                                                                                                                                                           |
| frozenBal            | String          | Frozen balance                                                                                                                                                                                                         |
| availBal             | String          | Avail balance                                                                                                                                                                                                          |
| lever                | String          | Leverage<br>Only applicable to `contract signal`                                                                                                                                                                       |
| investAmt            | String          | Investment amount                                                                                                                                                                                                      |
| subOrdType           | String          | Sub order type<br>`1`：limit order<br>`2`：market order<br>`9`：tradingView signal                                                                                                                                        |
| ratio                | String          | Price offset ratio, calculate the limit price as a percentage offset from the best bid/ask price<br>Only applicable to `subOrdType` is `limit order`                                                                   |
| entrySettingParam    | Object          | Entry setting                                                                                                                                                                                                          |
| > allowMultipleEntry | Boolean          | Whether or not allow multiple entries in the same direction for the same trading pairs                                                                                                                                 |
| > entryType          | String          | Entry type<br>`1`: TradingView signal<br>`2`: Fixed margin<br>`3`: Contracts<br>`4`: Percentage of free margin<br>`5`: Percentage of the initial invested margin                                                       |
| > amt                | String          | Amount per order<br>Only applicable to `entryType` in `2`/`3`                                                                                                                                                          |
| > ratio              | String          | Amount ratio per order<br>Only applicable to `entryType` in `4`/`5`                                                                                                                                                    |
| exitSettingParam     | Object          | Exit setting                                                                                                                                                                                                           |
| > tpSlType           | String          | Type of set the take-profit and stop-loss trigger price<br>`pnl`: Based on the estimated profit and loss percentage from the entry point<br>`price`: Based on price increase or decrease from the crypto’s entry price |
| > tpPct              | String          | Take-profit percentage                                                                                                                                                                                                 |
| > slPct              | String          | Stop-loss percentage                                                                                                                                                                                                   |
| signalChanId         | String          | Signal channel Id                                                                                                                                                                                                      |
| signalChanName       | String          | Signal channel name                                                                                                                                                                                                    |
| signalSourceType     | String          | Signal source type<br>`1`: Created by yourself<br>`2`: Subscribe<br>`3`: Free signal                                                                                                                                   |
