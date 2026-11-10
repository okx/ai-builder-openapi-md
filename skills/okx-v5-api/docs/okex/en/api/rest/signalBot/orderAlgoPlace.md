---
site: okex
lang: en
name: POST / Create signal bot
method: POST
path: /api/v5/tradingBot/signal/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Create signal bot

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/order-algo`

> Request Example

```shell
# Create signal bot
POST /api/v5/tradingBot/signal/order-algo
body
{
  "signalChanId": "627921182788161536",
  "instIds": [
    "BTC-USDT-SWAP",
    "ETH-USDT-SWAP",
    "LTC-USDT-SWAP"
  ],
  "lever": "10",
  "investAmt": "100",
  "subOrdType": "9",
  "entrySettingParam": {
    "allowMultipleEntry": true,
    "entryType": "1",
    "amt": "",
    "ratio": ""
  },
  "exitSettingParam": {
    "tpSlType": "2",
    "tpPct": "",
    "slPct": ""
  }
}
```

#### Request Parameters

| Parameter   | Type    | Required | Description                                                              |
|:------------| :------ |:---------|:-------------------------------------------------------------------------|
| signalChanId          | String           | Yes      | Signal channel Id                                                                                                            |
| lever                 | String           | Yes      | Leverage<br>Only applicable to `contract signal`                                                                 |
| investAmt             | String           | Yes      | Investment amount                                                                                                                      |
| subOrdType            | String           | Yes      | Sub order type `1`：limit order `2`：market order `9`：tradingView signal                                                                                        |
| includeAll            | Boolean          | No       | Whether to include all USDT-margined contract.The default value is `false`. `true`: include `false` : exclude                                                   |
| instIds               | String           | No       | Instrument IDs. Single currency or multiple currencies separated with comma. When `includeAll` is `true`, it is ignored                                        |
| ratio                 | String           | No       | Price offset ratio, calculate the limit price as a percentage offset from the best bid/ask price.<br>Only applicable to `subOrdType` is `limit` order                                                        |
| entrySettingParam     | String           | No       | Entry setting                                                                                                   |
| > allowMultipleEntry  | String           | No       | Whether or not allow multiple entries in the same direction for the same trading pairs.The default value is `true`。 `true`：Allow `false`：Prohibit                                                                                     |
| > entryType          | String          | No       | Entry type<br>`1`: TradingView signal<br>`2`: Fixed margin<br>`3`: Contracts<br>`4`: Percentage of free margin<br>`5`: Percentage of the initial invested margin                                                       |
| > amt                 | String           | No       | Amount per order <br>Only applicable to entryType in `2`/`3`                                                                                                                        |
| > ratio               | Array of objects  | No       | Amount ratio per order<br>Only applicable to entryType in `4`/`5`                                                                                                  |
| exitSettingParam      | String           | No       | Exit setting                                                                                         |
| > tpSlType            | String           | 是        | Type of set the take-profit and stop-loss trigger price <br>`pnl`: Based on the estimated profit and loss percentage from the entry point <br>`price`: Based on price increase or decrease from the crypto’s entry price        |
| > tpPct               | String           | No       | Take-profit percentage                                                                                                          |
| > slPct               | String           | No       | Stop-loss percentage      |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "447053782921515008",
            "sCode": "0",
            "sMsg": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId      | String   | Algo ID            |
| algoClOrdId | String   | Client-supplied Algo ID         |
| sCode       | String   | The code of the event execution result, 0 means success. |
| sMsg        | String   | The code of the event execution result, 0 means success. |
