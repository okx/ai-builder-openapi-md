---
site: my
lang: en
name: Get leverage estimated info
method: GET
path: /api/v5/account/adjust-leverage-info
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get leverage estimated info

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/adjust-leverage-info`

> Request Example

```shell
GET /api/v5/account/adjust-leverage-info?instType=MARGIN&mgnMode=isolated&lever=3&instId=BTC-USDT

```

#### Request Parameters

| Parameter  | Type   | Required    | Description                                                                                                                                            |
| :------ | :----- |:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType       | String | Yes         | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`                                                                                                     |
| mgnMode | String | Yes         | Margin mode<br>`isolated`<br>`cross`                                                                                                                  |
| lever | String | Yes         | Leverage                                                                                                                                               |
| instId | String | Conditional | Instrument ID, e.g. BTC-USDT<br>It is required for these scenarioes: `SWAP` and `FUTURES`, Margin isolation, Margin cross in `Futures mode`. |
| ccy | String | Conditional | Currency used for margin, e.g. BTC<br>It is required for isolated margin and cross margin in `Futures mode`, `Multi-currency margin` and `Portfolio margin`                          |
| posSide | String | No          | posSide<br>`net`: The default value<br>`long`<br>`short`                                                                  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "estAvailQuoteTrans": "",
            "estAvailTrans": "1.1398040558348279",
            "estLiqPx": "",
            "estMaxAmt": "10.6095865868904898",
            "estMgn": "0.0701959441651721",
            "estQuoteMaxAmt": "176889.6871254563042714",
            "estQuoteMgn": "",
            "existOrd": false,
            "maxLever": "10",
            "minLever": "0.01"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**      | **Type** | **Description**                                                                                                                                                                                                                                                                             |
|:-------------------| :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| estAvailQuoteTrans | String   | The estimated margin(in quote currency) can be transferred out under the corresponding leverage<br>For cross, it is the maximum quantity that can be transferred from the trading account.<br>For isolated, it is the maximum quantity that can be transferred from the isolated position<br>Only applicable to `MARGIN` |
| estAvailTrans      | String   | The estimated margin can be transferred out under the corresponding leverage.<br>For cross, it is the maximum quantity that can be transferred from the trading account.<br>For isolated, it is the maximum quantity that can be transferred from the isolated position<br> The unit is base currency for `MARGIN`<br> It is not applicable to the scenario when increasing leverage for isolated position under `FUTURES` and `SWAP`    |
| estLiqPx           | String   | The estimated liquidation price under the corresponding leverage. Only return when there is a position.                                                                                                                                                                                     |
| estMgn             | String   | The estimated margin needed by position under the corresponding leverage.<br>For the `MARGIN` position, it is margin in base currency                                                                                                                                                      |
| estQuoteMgn        | String   | The estimated margin (in quote currency) needed by position under the corresponding leverage                                                                                                                                                                                                |
| estMaxAmt          | String   | For `MARGIN`, it is the estimated maximum loan in base currency under the corresponding leverage<br>For `SWAP` and `FUTURES`, it is the estimated maximum quantity of contracts that can be opened under the corresponding leverage                                                        |
| estQuoteMaxAmt     | String   | The `MARGIN` estimated maximum loan in quote currency under the corresponding leverage.                                                                                                                                                                                                     |
| existOrd           | Boolean   | Whether there is pending orders <br>`true`<br>`false`                                                                                                                                                                                                                                     |
| maxLever           | String   | Maximum leverage                                                                                                                                                                                                                                                                            |
| minLever           | String   | Minimum leverage                                                                                                                                                                                                                                                                            |
