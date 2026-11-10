---
site: okex
lang: en
name: GET / Recurring buy sub orders
method: GET
path: /api/v5/tradingBot/recurring/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Recurring buy sub orders

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/recurring/sub-orders`

> Request Example

```shell
GET /api/v5/tradingBot/recurring/sub-orders?algoId=560516615079727104
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                                   |
| ordId    | String | No       | Sub order ID                                                   |
| after    | String | No       | Pagination of data to return records earlier than the requested `algoId`.  |
| before    | String | No       | Pagination of data to return records newer than the requested `algoId`.  |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0.045315",
            "algoClOrdId": "",
            "algoId": "560516615079727104",
            "algoOrdType": "recurring",
            "avgPx": "1765.4",
            "cTime": "1679911222200",
            "fee": "-0.0000317205",
            "feeCcy": "ETH",
            "instId": "ETH-USDC",
            "instType": "SPOT",
            "ordId": "560523524230717440",
            "ordType": "market",
            "px": "-1",
            "side": "buy",
            "state": "filled",
            "sz": "80",
            "tag": "",
            "tdMode": "",
            "uTime": "1679911222207"
        },
        {
            "accFillSz": "0.00071526",
            "algoClOrdId": "",
            "algoId": "560516615079727104",
            "algoOrdType": "recurring",
            "avgPx": "27961.6",
            "cTime": "1679911222189",
            "fee": "-0.000000500682",
            "feeCcy": "BTC",
            "instId": "BTC-USDC",
            "instType": "SPOT",
            "ordId": "560523524184580096",
            "ordType": "market",
            "px": "-1",
            "side": "buy",
            "state": "filled",
            "sz": "20",
            "tag": "",
            "tdMode": "",
            "uTime": "1679911222194"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                           |
| :------------ | :------- |:------------------------------------------------------------------------------------------|
| algoId      | String   | Algo ID                                                                                   |
| instType      | String   | Instrument type                                                                           |
| instId       | String   | Instrument ID                                                                             |
| algoOrdType       | String   | Algo order type<br>`recurring`: recurring buy                                             |
| ordId       | String   | Sub order ID                                                                              |
| cTime       | String   | Sub order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`       |
| uTime       | String   | Sub order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`       |
| tdMode       | String   | Sub order trade mode<br>Margin mode : `cross`<br>Non-Margin mode : `cash`                 |
| ordType       | String   | Sub order type<br>`market`: Market order<br>`manual_add_order`: Manual add investment order    |
| sz       | String   | Sub order quantity to buy or sell                                                         |
| state       | String   | Sub order state<br>`canceled`<br>`live`<br>`partially_filled`<br>`filled`<br>`cancelling` |
| side       | String   | Sub order side<br>`buy` `sell`                                                            |
| px       | String   | Sub order limit price<br>If it is a market order, "-1" will be return                      |
| fee       | String   | Sub order fee                                                                             |
| feeCcy       | String   | Sub order fee currency                                                                    |
| avgPx       | String   | Sub order average filled price                                                            |
| accFillSz       | String   | Sub order accumulated fill quantity                                                       |
| tag       | String   | Order tag                                                                                 |
| algoClOrdId                  | String | Client-supplied Algo ID                                                                                                                                                                                                                                                     |
