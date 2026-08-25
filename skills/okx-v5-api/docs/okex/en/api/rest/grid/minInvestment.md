---
site: okex
lang: en
name: POST / Compute min investment (public)
method: POST
path: /api/v5/tradingBot/grid/min-investment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Compute min investment (public)

Authentication is not required for this public endpoint.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`POST /api/v5/tradingBot/grid/min-investment`

> Request Example

```shell
POST /api/v5/tradingBot/grid/min-investment
body 
{
    "instId": "ETH-USDT",
    "algoOrdType":"grid",
    "gridNum": "50",
    "maxPx":"5000",
    "minPx":"3000",
    "runType":"1",
    "investmentData":[
        {
            "amt":"0.01",
            "ccy":"ETH"
        },
        {
            "amt":"100",
            "ccy":"USDT"
        }
    ]
}
```

#### Request Parameters

| Parameter         | Type             | Required | Description                                                                                                                                                               |
|:------------------|:-----------------|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId            | String           | Yes      | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                       |
| algoOrdType       | String           | Yes      | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                                                    |
| maxPx             | String           | Yes      | Upper price of price range                                                                                                                                                |
| minPx             | String           | Yes      | Lower price of price range                                                                                                                                                |
| gridNum           | String           | Yes      | Grid quantity                                                                                                                                                             |
| runType           | String           | Yes       | Grid type<br>`1`: Arithmetic, `2`: Geometric                                                                                                    |
| direction         | String           | Conditional | Contract grid type<br>`long`,`short`,`neutral`<br>Only applicable to `contract grid`                                                                                      |
| lever             | String           | Conditional | Leverage<br>Only applicable to `contract grid`                                                                                                                            |
| basePos           | Boolean          | No       | Whether or not open a position when the strategy activates<br>Default is `false`<br>Neutral contract grid should omit the parameter<br>Only applicable to `contract grid` |
| investmentType           | String          | No       | Investment type, only applicable to `grid`<br> `quote`<br>`base`<br>`dual` |
| triggerStrategy           | String          | No       | Trigger stragety, <br> `instant`<br>`price`<br>`rsi` |
| topUpAmt   | String | No       | Top up amount, only applicable to spot grid                                                                          |
| investmentData | Array of objects  | No       | Invest Data                                                                                                                                                               |
| > amt             | String           | Yes      | Invest amount                                                                                                                                                             |
| > ccy             | String           | Yes      | Invest currency                                                                                                                                                           |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
           "minInvestmentData": [  
               {
                   "amt":"0.1",
                   "ccy":"ETH"
               },
               {
                   "amt":"100",
                   "ccy":"USDT"
               }
           ],
           "singleAmt":"10"
       }
    ]
}
```

#### Response Parameters

| **Parameter**     | **Type**         | **Description**                                                                                                                       |
|:------------------|:-----------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| minInvestmentData | Array of objects  | Minimum invest Data                                                                                                                   |
| > amt             | String           | Minimum invest amount                                                                                                                 |
| > ccy             | String           | Minimum Invest currency                                                                                                               |
| singleAmt         | String  | Single grid trading amount<br>In terms of `spot grid`, the unit is `quote currency`<br>In terms of `contract grid`, the unit is `contract` |
