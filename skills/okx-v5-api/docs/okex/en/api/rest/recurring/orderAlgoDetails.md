---
site: okex
lang: en
name: GET / Recurring buy order details
method: GET
path: /api/v5/tradingBot/recurring/orders-algo-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Recurring buy order details

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/tradingBot/recurring/orders-algo-details`

> Request Example

```shell
GET /api/v5/tradingBot/recurring/orders-algo-details?algoId=644497312047435776
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId    | String | Yes       | Algo ID                                                   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "644497312047435776",
            "algoOrdType": "recurring",
            "amt": "100",
            "cTime": "1699932133373",
            "cycles": "6",
            "instType": "SPOT",
            "investmentAmt": "0",
            "investmentCcy": "USDC",
            "mktCap": "0",
            "nextInvestTime": "1699956005500",
            "period": "hourly",
            "pnlRatio": "0",
            "recurringDay": "",
            "recurringHour": "1",
            "recurringList": [
                {
                    "avgPx": "0",
                    "ccy": "BTC",
                    "profit": "0",
                    "px": "36683.2",
                    "ratio": "0.2",
                    "minPx": "",
                    "maxPx": "",
                    "totalAmt": "0"
                },
                {
                    "avgPx": "0",
                    "ccy": "ETH",
                    "profit": "0",
                    "px": "2058.36",
                    "ratio": "0.8",
                    "minPx": "",
                    "maxPx": "",
                    "totalAmt": "0"
                }
            ],
            "recurringTime": "12",
            "state": "running",
            "stgyName": "stg1",
            "tag": "",
            "timeZone": "8",
            "totalAnnRate": "0",
            "totalPnl": "0",
            "uTime": "1699952485451",
            "tradeQuoteCcy": "USDT"，
            "source": ["1"],
            "recurringTimeType": "1",
            "recurringTimeMinutes": "0"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**         | **Type**        | **Description**                                                                                                                                                                                                              |
|:----------------------|:----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId                | String          | Algo ID                                                                                                                                                                                                                      |
| algoClOrdId           | String          | Client-supplied Algo ID                                                                                                                                                                                                      |
| instType              | String          | Instrument type                                                                                                                                                                                                              |
| cTime                 | String          | Algo order created time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                         |
| uTime                 | String          | Algo order updated time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                         |
| algoOrdType           | String          | Algo order type<br>`recurring`: recurring buy                                                                                                                                                                                |
| state                 | String          | Algo order state<br>`running`<br>`stopping`<br>`stopped`<br/>`pause`                                                                                                                                                                     |
| stgyName              | String          | Custom name for trading bot, no more than 40 characters                                                                                                                                                                      |
| recurringList         | Array of objects | Recurring buy info                                                                                                                                                                                                           |
| > ccy                 | String          | Recurring buy currency, e.g. `BTC`                                                                                                                                                                                           |
| > ratio               | String          | Proportion of recurring currency assets, e.g. "0.2" representing 20%                                                                                                                                                         |
| > minPx               | String          | Minimum price of recurring currency. `""` means no limit                                                                                                                                                                                     |
| > maxPx               | String          | Maximum price of recurring currency. `""` means no limit                                                                                                                                                                                     |
| > totalAmt            | String          | Accumulated quantity in unit of recurring buy currency                                                                                                                                                                       |
| > profit              | String          | Profit in unit of `investmentCcy`                                                                                                                                                                                            |
| > avgPx               | String          | Average price of recurring buy, quote currency is `investmentCcy`                                                                                                                                                            |
| > px                  | String          | Current market price, quote currency is `investmentCcy`                                                                                                                                                                      |
| period                | String          | Period<br>`monthly`<br>`weekly`<br>`daily`<br>`hourly`                                                                                                                                                                       |
| recurringDay          | String          | Recurring buy date<br>When the period is `monthly`, the value range is an integer of [1,28]<br>When the period is `weekly`, the value range is an integer of [1,7]                                                           |
| recurringHour         | String          | Recurring buy by hourly<br>`1`/`4`/`8`/`12`, e.g. `4` represents "recurring buy every 4 hour"                                                                                                                                |
| recurringTime         | String          | Recurring buy time, the value range is an integer of [0,23]                                                                                                                                                                  |
| timeZone              | String          | UTC time zone, the value range is an integer of [-12,14]<br>e.g. "8" representing UTC+8 (East 8 District), Beijing Time                                                                                                      |
| amt                   | String          | Quantity invested per cycle                                                                                                                                                                                                  |
| investmentAmt         | String          | Accumulate quantity invested                                                                                                                                                                                                 |
| investmentCcy         | String          | The invested quantity unit, can only be `USDT`/`USDC`                                                                                                                                                                        |
| nextInvestTime        | String          | Next invest time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                |
| totalPnl              | String          | Total P&L                                                                                                                                                                                                                    |
| totalAnnRate          | String          | Total annualized rate of yield                                                                                                                                                                                               |
| pnlRatio              | String          | Rate of yield                                                                                                                                                                                                                |
| mktCap                | String          | Market value in unit of `USDT`                                                                                                                                                                                               |
| cycles                | String          | Accumulate recurring buy cycles                                                                                                                                                                                              |
| tag                   | String          | Order tag                                                                                                                                                                                                                    |
| tradeQuoteCcy   | String   | The quote currency for trading. |
| source               | Array           | Funding source<br>`1`: Trading account<br>`2`: Funding account<br>`3`: Simple earn account                                                                                                                                                   |
| recurringTimeType    | String          | Recurring buy time type<br>`1`: Custom time<br>`2`: Immediate trigger                                                                                                                                                                        |
| recurringTimeMinutes | String          | Recurring buy time in minutes, integer of [0,59]                                                                                                                                                                                             |
