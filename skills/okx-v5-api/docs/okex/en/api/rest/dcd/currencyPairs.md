---
site: okex
lang: en
name: GET / Currency pairs
method: GET
path: /api/v5/finance/sfp/dcd/currency-pair
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

## Dual investment
### GET / Currency pairs

Returns available dual investment currency pairs.

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/sfp/dcd/currency-pair`

> Request Example

```shell
GET /api/v5/finance/sfp/dcd/currency-pair
```

#### Request Parameters

None

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "baseCcy": "BTC",
            "quoteCcy": "USDT",
            "optType": "C",
            "uly": "BTC-USD"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| baseCcy | String | Base currency |
| quoteCcy | String | Quote currency |
| optType | String | Option type<br>`C`: Call<br>`P`: Put |
| uly | String | Underlying |
