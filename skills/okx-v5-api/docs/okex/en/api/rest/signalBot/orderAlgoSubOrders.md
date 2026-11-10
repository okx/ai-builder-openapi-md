---
site: okex
lang: en
name: GET / Signal bot sub orders
method: GET
path: /api/v5/tradingBot/signal/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Signal bot sub orders

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/signal/sub-orders`

> Request Example

```shell
# Get historical filled sub orders
GET /api/v5/tradingBot/signal/sub-orders?algoId=623833708424069120&algoOrdType=contract&state=filled

# Get designated sub order
GET /api/v5/tradingBot/signal/sub-orders?algoId=623833708424069120&algoOrdType=contract&signalOrdId=O632302662327996418
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                      |
| :--------- | :------ | :------- |:-------------------------------------------------------------------------------------------------|
| algoId      | String | Yes    | Algo ID                                                                                                                           |
| algoOrdType | String | Yes    | Algo order type<br>`contract`: Contract signal                                                                                    |
| state       | String | Conditional     | Sub order state<br>`live`<br>`partially_filled`<br>`filled`<br>`cancelled`<br>Either `state` or `signalOrdId` is required, if both are passed in, only `state` is valid.                       |
| signalOrdId | String | Conditional     | Sub order ID                                                                                                                      |
| after       | String | No    | Pagination of data to return records earlier than the requested `ordId`                                                            |
| before      | String | No    | Pagination of data to return records newer than the requested `ordId`.                                                             |
| begin       | String | No    | Return records of `ctime` after than the requested timestamp (include), Unix timestamp format in milliseconds, e.g. `1597026383085`|
| end         | String | No    | Return records of `ctime` before than the requested timestamp (include), Unix timestamp format in milliseconds, e.g. `1597026383085`|
| limit       | String | No    | Number of results per request. The maximum is 100. The default is 100.                                                             |
| type        | String | No     | Sub order type <br>`live`<br>`filled`<br>Either `type` or `clOrdId` is required, if both are passed in, only `clOrdId` is valid.  |
| clOrdId     | String | No     | Sub order client-supplied ID. <br>`It will be deprecated soon`                                                                   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "18",
            "algoClOrdId": "",
            "algoId": "623833708424069120",
            "algoOrdType": "contract",
            "avgPx": "1572.81",
            "cTime": "1697024702320",
            "ccy": "",
            "clOrdId": "O632302662327996418",
            "ctVal": "0.01",
            "fee": "-0.1415529",
            "feeCcy": "USDT",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "lever": "10",
            "ordId": "632302662351958016",
            "ordType": "market",
            "pnl": "-2.6784",
            "posSide": "net",
            "px": "",
            "side": "buy",
            "state": "filled",
            "sz": "18",
            "tag": "",
            "tdMode": "cross",
            "uTime": "1697024702322"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type**    | **Description**                                                                           |
|:----------------|:------------|:------------------------------------------------------------------------------------------|
| algoId          | String      | Algo ID                                                                                   |
| algoClOrdId     | String      | Client-supplied Algo ID. Used to be extended in the future                                                                     |
| instType        | String      | Instrument type                                                                           |
| instId          | String      | Instrument ID                                                                             |
| algoOrdType     | String      | Algo order type<br>`contract`: Contract signal                                            |
| ordId           | String      | Sub order ID                                                                              |
| clOrdId         | String      | Sub order client-supplied ID. <br> It is equal to `signalOrdId`                                                               |
| cTime           | String      | Sub order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`       |
| uTime           | String      | Sub order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`       |
| tdMode          | String      | Sub order trade mode<br>Margin mode: `cross`/`isolated`<br>Non-Margin mode: `cash`        |
| ccy             | String      | Margin currency<br>Only applicable to cross MARGIN orders in `Futures mode`.      |
| ordType         | String      | Sub order type<br>`market`: Market order<br>`limit`: Limit order<br>`ioc`: Immediate-or-cancel order                          |
| sz              | String      | Sub order quantity to buy or sell                                                         |
| state           | String      | Sub order state<br>`canceled`<br>`live`<br>`partially_filled`<br>`filled`<br>`cancelling` |
| side            | String      | Sub order side<br>`buy`,`sell`                                                            |
| px              | String      | Sub order price                                                                           |
| fee             | String      | Sub order fee amount                                                                      |
| feeCcy          | String      | Sub order fee currency                                                                    |
| avgPx           | String      | Sub order average filled price                                                            |
| accFillSz       | String      | Sub order accumulated fill quantity                                                       |
| posSide         | String      | Sub order position side<br>`net`                                                          |
| pnl             | String      | Sub order profit and loss                                                                 |
| ctVal           | String      | Contract value<br>Only applicable to `FUTURES`/`SWAP`                                       |
| lever           | String      | Leverage                                                                                  |
| tag             | String      | Order tag                                                                                 |
