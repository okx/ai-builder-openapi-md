---
site: hongkong
lang: en
name: Get convert currency pair
method: GET
path: /api/v5/asset/convert/currency-pair
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get convert currency pair

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/convert/currency-pair`

> Request Example

```shell
GET /api/v5/asset/convert/currency-pair?fromCcy=USDT&toCcy=BTC

```

#### Response parameters

| **Parameters** | **Types** | **Required** | **Description**  |
| :--- | :----- | :------- | :---------------------- |
| fromCcy  | String | Yes    | Currency to convert from, e.g. `USDT`  |
| toCcy  | String | Yes   | Currency to convert to, e.g. `BTC`  |
| convertMode | String | No | `0`: standard convert (default) <br> `1`: large order convert for VIP |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "baseCcy": "BTC",
            "baseCcyMax": "0.5",
            "baseCcyMin": "0.0001",
            "instId": "BTC-USDT",
            "quoteCcy": "USDT",
            "quoteCcyMax": "10000",
            "quoteCcyMin": "1"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type    | Description                                                         |
| ----------- | ------- | ------------------------------------------------------------------------------------------- |
| instId         | String  | Currency pair, e.g. `BTC-USDT`                                                                          |
| baseCcy        | String  | Base currency, e.g. `BTC` in `BTC-USDT`                                                         |
| baseCcyMax     | String  | Maximum amount of base currency                                                                    |
| baseCcyMin     | String  | Minimum amount of base currency                                                                     |
| quoteCcy       | String  | Quote currency, e.g. `USDT` in `BTC-USDT`                                                             |
| quoteCcyMax    | String  | Maximum amount of quote currency                                                                     |
| quoteCcyMin    | String  | Minimum amount of quote currency                                                                      |
