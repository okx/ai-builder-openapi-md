---
site: my
lang: en
name: Get buy/sell currency pair
method: GET
path: /api/v5/fiat/buy-sell/currency-pair
module: fiat
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get buy/sell currency pair

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/buy-sell/currency-pair`

> Request Example

```shell
GET /api/v5/fiat/buy-sell/currency-pair?fromCcy=USD&toCcy=BTC
```

#### Request Parameters

| Parameters | Types  | Required | Description                  |
|:----------|:-------|:---------|:-----------------------------|
| fromCcy    | String | Yes      | Currency to sell, e.g. `USD` |
| toCcy      | String | Yes      | Currency to buy, e.g. `BTC`  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "singleTradeMax": "1",
            "singleTradeMin": "0.01",
            "fixedPxRemainingDailyQuota": "", 
            "fixedPxDailyLimit": "", 
            "paymentMethods":["balance"]
        }
    ],
    "msg": ""
}

{
    "code": "0",
    "data": [
        {
            "side": "sell",
            "fromCcy": "BTC",
            "toCcy": "USD",
            "singleTradeMax": "1",
            "singleTradeMin": "0.01",
            "fixedPxRemainingDailyQuota": "", 
            "fixedPxDailyLimit": "", 
            "paymentMethods":["balance"]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter                  | Type              | Description                                                                                                                        |
|----------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| side                       | String            | Side<br>`buy`: Fiat to crypto<br>`sell`: Crypto to fiat<br>May support both sides in the future, separated with a comma, e.g. `buy,sell`. |
| fromCcy                    | String            | Currency to sell, e.g. `USD`                                                                                           |
| toCcy                      | String            | Currency to buy, e.g. `BTC`                                                                                            |
| singleTradeMax             | String            | The maximum amount of currency for a single trade, unit in `fromCcy`                                                   |
| singleTradeMin             | String            | The minimum amount of currency for a single trade, unit in `fromCcy`                                                   |
| fixedPxDailyLimit          | String            | Fixed price daily limit<br>Applicable to Fiat to Fiat trade, else return ''.<br>If `side` = `buy`, unit in `fromCcy`<br>If `side` = `sell`, unit in `toCcy` |
| fixedPxRemainingDailyQuota | String            | Fixed price remaining daily quota<br>Applicable to Fiat to Fiat trade, else return ''.<br>If `side` = `buy`, unit in `fromCcy`<br>If `side` = `sell`, unit in `toCcy` |
| paymentMethods             | Array of strings  | Supported payment methods<br>`balance`<br>e.g. ["balance"]                                                          |

    <aside class="notice">
        This feature is only available to EU users at the moment.
    </aside>
