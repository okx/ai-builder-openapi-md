---
site: okex
lang: en
name: GET / Max grid quantity (public)
method: GET
path: /api/v5/tradingBot/grid/grid-quantity
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Max grid quantity (public)

Authentication is not required for this public endpoint.<br>

Maximum grid quantity can be retrieved from this endpoint. Minimum grid quantity always is 2.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/tradingBot/grid/grid-quantity`

> Request Example

```shell
GET /api/v5/tradingBot/grid/grid-quantity?instId=BTC-USDT-SWAP&runType=1&algoOrdType=contract_grid&maxPx=70000&minPx=50000&lever=5
```

#### Request Parameters

| Parameter   | Type      | Required   | Description                                                                                                                                                    |
|:------------|:----------|:-----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId      | String    | Yes        | Instrument ID, e.g. `BTC-USDT`                                                                                                    |
| runType   | String    | Yes        | Grid type<br>`1`: Arithmetic<br> `2`: Geometric|
| algoOrdType | String | Yes       | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                           |
| maxPx       | String   | Yes       | Upper price of price range           |
| minPx       | String   | Yes       | Lower price of price range           |
| lever | String    | Conditional         | Leverage, it is required for contract grid                                                   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "maxGridQty": "285"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                    |
|:----------------| :------- |:-----------------------------------|
| maxGridQty      | String   | Maximum grid quantity |
