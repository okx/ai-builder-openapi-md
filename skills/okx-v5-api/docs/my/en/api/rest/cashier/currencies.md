---
site: my
lang: en
name: Get buy/sell currencies
method: GET
path: /api/v5/fiat/buy-sell/currencies
module: fiat
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get buy/sell currencies

#### Rate Limit: 6 requests per second 
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/buy-sell/currencies`

> Request Example

```shell
GET /api/v5/fiat/buy-sell/currencies
```

> Response Example

```json
{
    "code": "0",
    "data": [
        {
           "fiatCcyList":[
                {
                    "ccy": "USD"
                },
                {
                    "ccy": "EUR"
                },
                ...
            ],
            "cryptoCcyList":[
                {
                    "ccy": "BTC"
                },
                ...
            ],
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter      | Type              | Description            |
|:--------------|:------------------|:-----------------------|
| fiatCcyList    | Array of objects  | Fiat currency list     |
| >ccy           | String            | Currency, e.g. `BTC`   |
| cryptoCcyList  | Array of objects  | Crypto currency list   |
| >ccy           | String            | Currency, e.g. `USD`   |

    <aside class="notice">
        This feature is only available to EU users at the moment.
    </aside>
