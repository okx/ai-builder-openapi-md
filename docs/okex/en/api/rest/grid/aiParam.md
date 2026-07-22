---
site: okex
lang: en
name: GET / Grid AI parameter (public)
method: GET
path: /api/v5/tradingBot/grid/ai-param
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Grid AI parameter (public)

Authentication is not required for this public endpoint.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/tradingBot/grid/ai-param`

> Request Example

```shell
GET /api/v5/tradingBot/grid/ai-param?instId=BTC-USDT&algoOrdType=grid
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                                                |
| :--------- | :------ | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoOrdType | String | Yes       | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                           |
| instId    | String | Yes       | Instrument ID, e.g. `BTC-USDT`                                                                                                                                             |
| direction | String | Conditional   | Contract grid type<br>`long`,`short`,`neutral`<br>Required in the case of `contract_grid`                                                                                  |
| duration | String | No | Back testing duration in number of days<br>Spot grid default is `7D` with available durations of `7D`, `30D` and `180D`<br>Contract grid default is `14D` with available durations of `7D`, `14D` and `30D` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoOrdType": "grid",
            "annualizedRate": "1.5849",
            "ccy": "USDT",
            "direction": "",
            "duration": "7D",
            "gridNum": "5",
            "instId": "BTC-USDT",
            "lever": "0",
            "maxPx": "21373.3",
            "minInvestment": "0.89557758",
            "minPx": "15544.2",
            "perGridProfitRatio": "4.566226200302574",
            "perMaxProfitRate": "0.0733865364573281",
            "perMinProfitRate": "0.0561101403446263",
            "runType": "1",
            "sourceCcy": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                    |
| :------------ | :------- |:-----------------------------------------------------------------------------------|
| instId       | String   | Instrument ID, e.g. BTC-USDT-SWAP                                                  |
| algoOrdType       | String   | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid             |
| duration       | String   | Back testing duration<br>`7D`: 7 Days, `30D`: 30 Days, `180D`: 180 Days            |
| gridNum       | String   | Grid quantity                                                                      |
| maxPx       | String   | Upper price of price range                                                         |
| minPx       | String   | Lower price of price range                                                         |
| perMaxProfitRate       | String   | Estimated maximum Profit margin per grid                                           |
| perMinProfitRate       | String   | Estimated minimum Profit margin per grid                                           |
| perGridProfitRatio       | String   | Per grid profit ratio                                                              |
| annualizedRate       | String   | Grid annualized rate                                                               |
| minInvestment       | String   | The minimum invest amount                                                          |
| ccy       | String   | The invest currency                                                                |
| runType       | String   | Grid type<br>`1`: Arithmetic, `2`: Geometric                                       |
| direction       | String   | Contract grid type<br>`long`,`short`,`neutral`<br>Only applicable to contract grid |
| lever       | String   | Leverage<br>Only applicable to contract grid                                       |
| sourceCcy       | String   | Source currency                                                                    |
