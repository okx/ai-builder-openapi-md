---
site: okex
lang: en
name: GET / Grid algo order positions
method: GET
path: /api/v5/tradingBot/grid/positions
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Grid algo order positions

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/grid/positions`

> Request Example

```shell
GET /api/v5/tradingBot/grid/positions?algoId=448965992920907776&algoOrdType=contract_grid
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                       |
| :--------- | :------ | :------- |:--------------------------------------------------|
| algoOrdType | String | Yes       | Algo order type<br>`contract_grid`: Contract grid |
| algoId    | String | Yes       | Algo ID                                           |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "algoClOrdId": "",
            "algoId": "449327675342323712",
            "avgPx": "29215.0142857142857149",
            "cTime": "1653400065917",
            "ccy": "USDT",
            "imr": "2045.386",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "last": "29206.7",
            "lever": "5",
            "liqPx": "661.1684795867162",
            "markPx": "29213.9",
            "mgnMode": "cross",
            "mgnRatio": "217.19370606167573",
            "mmr": "40.907720000000005",
            "notionalUsd": "10216.70307",
            "pos": "35",
            "posSide": "net",
            "uTime": "1653400066938",
            "upl": "1.674999999999818",
            "uplRatio": "0.0008190504784478"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                              |
|:--------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                                                                      |
| algoClOrdId   | String   | Client-supplied Algo ID                                                                                                      |
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
