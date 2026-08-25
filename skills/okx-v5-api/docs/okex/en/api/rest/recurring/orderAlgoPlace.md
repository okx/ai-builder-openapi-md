---
site: okex
lang: en
name: POST / Place recurring buy order
method: POST
path: /api/v5/tradingBot/recurring/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Place recurring buy order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/recurring/order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/order-algo
body
{
  "stgyName": "BTC|ETH recurring buy monthly",     
  "amt":"100",
  "recurringList":[    
    {
         "ccy":"BTC",
         "ratio":"0.2"
    },
    {
         "ccy":"ETH",
         "ratio":"0.8"
    }
  ],
  "period":"monthly",
  "recurringDay":"1",
  "recurringTime":"0",
  "timeZone":"8",   // UTC +8
  "tdMode":"cross",
  "investmentCcy":"USDT"
}
```

#### Request Parameters

| Parameter     | Type    | Required           | Description                                                                                                                                                                                                                               |
|:--------------| :------ |:-------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| stgyName      | String | Yes                | Custom name for trading bot, no more than 40 characters                                                                                                                                                                                   |
| recurringList | Array of objects | Yes                | Recurring buy info                                                                                                                                                                                                                        |
| > ccy         | String | Yes                | Recurring currency, e.g. `BTC`                                                                                                                                                                                                            |
| > ratio       | String | Yes                | Proportion of recurring currency assets, e.g. "0.2" representing 20%                                                                                                                                                                      |
| > minPx       | String | No                 | Minimum price of recurring currency. `""` means no limit                                                                                                                                                                                  |
| > maxPx       | String | No                 | Maximum price of recurring currency. `""` means no limit                                                                                                                                                                                  |
| period        | String | Yes                | Period<br>`monthly`<br>`weekly`<br>`daily`<br>`hourly`                                                                                                                                                                                    |
| recurringDay  | String | Conditional        | Recurring buy date<br>When the period is `monthly`, the value range is an integer of [1,28]<br>When the period is `weekly`, the value range is an integer of [1,7]<br>When the period is `daily`/`hourly`, the parameter is not required. |
| recurringHour | String | Conditional        | Recurring buy by hourly<br>`1`/`4`/`8`/`12`, e.g. `4` represents "recurring buy every 4 hour"<br>When the period is `hourly`, the parameter is required.                                                                                  |
| recurringTime | String | Yes                | Recurring buy time, the value range is an integer of [0,23]<br>When the period is `hourly`, the parameter is the time of the first investment occurs.                                                                                     |
| timeZone      | String | Yes                | UTC time zone, the value range is an integer of [-12,14]<br>e.g. "8" representing UTC+8 (East 8 District), Beijing Time                                                                                                                   |
| amt           | String | Yes                | Quantity invested per cycle                                                                                                                                                                                                               |
| investmentCcy | String | Yes                | The invested quantity unit, can only be `USDT`/`USDC`                                                                                                                                                                                     |
| tdMode        | String | Yes                | Trading mode<br>Margin mode: `cross`<br>Non-Margin mode: `cash`                                                                                                                                                                           |
| algoClOrdId   | String | No                 | Client-supplied Algo ID<br>There will be a value when algo order attaching algoClOrdId is triggered, or it will be "".<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.              |
| tag           | String | No                 | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                           |
| tradeQuoteCcy | String | No                 | The quote currency for trading.   
| source        | Array  | No                 | Funding source<br>`1`: Trading account<br>`2`: Funding account<br>`3`: Simple earn account<br>Default is `1`                                                                                                                              |
| recurringTimeType | String | No             | Recurring buy time type<br>`1`: Custom time<br>`2`: Immediate trigger<br>Default is `1`                                                                                                                                                   |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"560472804207104000",
            "algoClOrdId":"",
            "sCode":"0",
            "sMsg":"",
            "tag":""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                |
| :------------ | :------- |:-----------------------------------------------------------------------------------------------|
| algoId      | String   | Algo ID                                                                                        |
| algoClOrdId      | String   | Client-supplied Algo ID                                                                        |
| sCode      | String   | The code of the event execution result, 0 means success                                        |
| sMsg       | String   | Rejection message if the request is unsuccessful                                               |
| tag       | String   | Order tag                                               |
