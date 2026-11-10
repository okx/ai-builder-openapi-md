---
site: okex
lang: en
name: GET / Grid algo sub orders
method: GET
path: /api/v5/tradingBot/grid/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Grid algo sub orders

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/grid/sub-orders`

> Request Example

```shell
GET /api/v5/tradingBot/grid/sub-orders?algoId=123456&type=live&algoOrdType=grid
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoOrdType | String | Yes       | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid   |
| algoId    | String | Yes       | Algo ID                                                   |
| type | String | Yes       | Sub order state<br>`live`<br>`filled`  |
| groupId | String | No       | Group ID  |
| after    | String | No       | Pagination of data to return records earlier than the requested `ordId`.  |
| before    | String | No       | Pagination of data to return records newer than the requested `ordId`.  |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "algoOrdType": "grid",
            "avgPx": "0",
            "cTime": "1653347949771",
            "ccy": "",
            "ctVal": "",
            "fee": "0",
            "feeCcy": "USDC",
            "groupId": "3",
            "instId": "BTC-USDC",
            "instType": "SPOT",
            "lever": "0",
            "ordId": "449109084439187456",
            "ordType": "limit",
            "pnl": "0",
            "posSide": "net",
            "px": "30404.3",
            "rebate": "0",
            "rebateCcy": "USDT",
            "side": "sell",
            "state": "live",    
            "sz": "0.00059213",
            "tag": "",
            "tdMode": "cash",
            "uTime": "1653347949831"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                  |
|:--------------| :------- |:-------------------------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                                          |
| algoClOrdId   | String   | Client-supplied Algo ID                                                                          |
| instType      | String   | Instrument type                                                                                  |
| instId        | String   | Instrument ID                                                                                    |
| algoOrdType   | String   | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid |
| groupId       | String   | Group ID                                                                                         |
| ordId         | String   | Sub order ID                                                                                     |
| cTime         | String   | Sub order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`              |
| uTime         | String   | Sub order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`              |
| tdMode        | String   | Sub order trade mode<br>Margin mode: `cross`/`isolated`<br>Non-Margin mode: `cash`               |
| ccy           | String   | Margin currency<br>Only applicable to cross MARGIN orders in `Futures mode`.             |
| ordType       | String   | Sub order type<br>`market`: Market order<br>`limit`: Limit order<br>`ioc`: Immediate-or-cancel order                                 |
| sz            | String   | Sub order quantity to buy or sell                                                                |
| state         | String   | Sub order state<br>`canceled`<br>`live`<br>`partially_filled`<br>`filled`<br>`cancelling`        |
| side          | String   | Sub order side<br>`buy` `sell`                                                                   |
| px            | String   | Sub order price                                                                                  |
| fee           | String   | Sub order fee amount                                                                             |
| feeCcy        | String   | Sub order fee currency                                                                           |
| rebate        | String   | Sub order rebate amount                                                                          |
| rebateCcy     | String   | Sub order rebate currency                                                                        |
| avgPx         | String   | Sub order average filled price                                                                   |
| accFillSz     | String   | Sub order accumulated fill quantity                                                              |
| posSide       | String   | Sub order position side<br>`net`                                                  |
| pnl           | String   | Sub order profit and loss                                                                        |
| ctVal         | String   | Contract value<br>Only applicable to `FUTURES`/`SWAP`                                              |
| lever         | String   | Leverage                                                                                         |
| tag           | String   | Order tag                                                                                        |
