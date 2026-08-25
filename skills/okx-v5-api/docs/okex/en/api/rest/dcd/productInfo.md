---
site: okex
lang: en
name: GET / Product info
method: GET
path: /api/v5/finance/sfp/dcd/products
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Product info

Return dual investment product list.

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/sfp/dcd/products`

> Request Example

```shell
GET /api/v5/finance/sfp/dcd/products?baseCcy=BTC&quoteCcy=USDT&optType=C
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| baseCcy | String | Yes | Base currency |
| quoteCcy | String | Yes | Quote currency |
| optType | String | Yes | Option type<br>`C`: Call<br>`P`: Put |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "absYield": "0.00232413",
            "annualizedYield": "0.0541",
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "expTime": "1774598400000",
            "interestAccrualTime": "1773244800000",
            "listTime": "1743150759000",
            "maxSize": "6000000",
            "minSize": "10",
            "notionalCcy": "USDT",
            "optType": "P",
            "productId": "BTC-USDT-260327-54500-P",
            "quoteTime": "1773243808703",
            "redeemEndTime": "1774594800000",
            "redeemStartTime": "1773244800000",
            "stepSz": "1",
            "tradeEndTime": "1774584000000",
            "strike": "54500",
            "uly": "BTC-USD"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| absYield | String | Absolute yield |
| annualizedYield | String | Annualized yield |
| baseCcy | String | Base currency |
| quoteCcy | String | Quote currency |
| notionalCcy | String | Investment currency. If `C`, then baseCcy; if `P`, then quoteCcy. |
| expTime | String | Expiry time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| interestAccrualTime | String | Interest accrual start time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| listTime | String | Product launch time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| minSize | String | Minimum trade size in notional currency |
| maxSize | String | Maximum trade size in notional currency |
| optType | String | Option type<br>`C`: Call<br>`P`: Put |
| productId | String | Product ID |
| quoteTime | String | When product was quoted, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| redeemStartTime | String | Earliest time to request early redemption, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| redeemEndTime | String | Latest time to request early redemption, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| stepSz | String | Trade step size in notional currency |
| tradeEndTime | String | Trade end time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uly | String | Underlying |
| strike | String | Strike price |
