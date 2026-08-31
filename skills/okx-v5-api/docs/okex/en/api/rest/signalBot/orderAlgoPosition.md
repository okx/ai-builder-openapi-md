---
site: okex
lang: en
name: GET / Signal bot order positions
method: GET
path: /api/v5/tradingBot/signal/positions
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Signal bot order positions

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/positions`

> Request Example

```shell
GET /api/v5/tradingBot/signal/positions?algoId=623833708424069120&algoOrdType=contract
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- |:-----------------------------------------------|
| algoOrdType | String | Yes       | Algo order type<br>`contract`: Contract signal |
| algoId    | String | Yes       | Algo ID                                        |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "algoClOrdId": "",
            "algoId": "623833708424069120",
            "avgPx": "1597.74",
            "cTime": "1697502301460",
            "ccy": "USDT",
            "imr": "23.76495",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "last": "1584.34",
            "lever": "10",
            "liqPx": "1438.7380360728976",
            "markPx": "1584.33",
            "mgnMode": "cross",
            "mgnRatio": "11.719278420807477",
            "mmr": "1.9011959999999997",
            "notionalUsd": "237.75168928499997",
            "pos": "15",
            "posSide": "net",
            "uTime": "1697502301460",
            "upl": "-2.0115000000000123",
            "uplRatio": "-0.0839310526118142"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                              |
|:--------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                                                                      |
| algoClOrdId   | String   | Client-supplied Algo ID. Used to be extended in the future.                                                                                                   |
| instType      | String   | Instrument type                                                                                                              |
| instId        | String   | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                           |
| cTime         | String   | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                         |
| uTime         | String   | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                         |
| avgPx         | String   | Average open price                                                                                                           |
| ccy           | String   | Margin currency                                                                                                              |
| lever         | String   | Leverage                                                                                                                     |
| liqPx         | String   | Estimated liquidation price                                                                                                  |
| posSide       | String   | Position side<br>`net`                                                                                                       |
| pos           | String   | Quantity of positions                                                                                                        |
| mgnMode       | String   | Margin mode<br>`cross`<br>`isolated`                                                                                         |
| mgnRatio      | String   | Maintenance margin ratio                                                                                                                 |
| imr           | String   | Initial margin requirement                                                                                                   |
| mmr           | String   | Maintenance margin requirement                                                                                               |
| upl           | String   | Unrealized profit and loss                                                                                                   |
| uplRatio      | String   | Unrealized profit and loss ratio                                                                                             |
| last          | String   | Latest traded price                                                                                                          |
| notionalUsd   | String   | Notional value of positions in `USD`                                                                                         |
| adl           | String   | Automatic-Deleveraging, signal area<br>Divided into 5 levels, from 1 to 5, the smaller the number, the weaker the adl intensity. |
| markPx        | String   | Mark price                                                                                                                   |
