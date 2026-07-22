---
site: okex
lang: en
name: GET / Option trades
method: GET
path: /api/v5/public/option-trades
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Option trades

The maximum is 100.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/option-trades`

> Request Example

```shell
GET /api/v5/public/option-trades?instFamily=BTC-USD
```

#### Request Parameters

| Parameter      | Type   | Required    | Description                                                                                                                        |
|:---------------| :----- |:------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| instId       | String | Conditional | Instrument ID, e.g. BTC-USD-221230-4000-C, Either `instId` or `instFamily` is required. If both are passed, `instId` will be used. |
| instFamily         | String | Conditional          | Instrument family, e.g. BTC-USD                                                                                                    |
| optType            | String | No          | Option type, `C`: Call  `P`: put                                                                                                            |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fillVol": "0.24415013671875",
            "fwdPx": "16676.907614127158",
            "idxPx": "16667",
            "instFamily": "BTC-USD",
            "instId": "BTC-USD-221230-16600-P",
            "markPx": "0.006308943261227884",
            "optType": "P",
            "px": "0.005",
            "side": "sell",
            "sz": "30",
            "tradeId": "65",
            "ts": "1672225112048"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                          |
|:--------------| :------- |:-------------------------------------------------------------------------|
| instId        | String   | Instrument ID                                                            |
| instFamily    | String   | Instrument family                                                        |
| tradeId       | String   | Trade ID                                                                 |
| px            | String   | Trade price                                                              |
| sz         | String   | Trade quantity. The unit is contract.                                  |
| side          | String   | Trade side <br>`buy`  <br>`sell`                                        |
| optType       | String   | Option type, C: Call P: Put                                              |
| fillVol       | String   | Implied volatility while trading (Correspond to trade price)             |
| fwdPx         | String   | Forward price while trading                                              |
| idxPx         | String   | Index price while trading                                                |
| markPx        | String   | Mark price while trading                                                 |
| ts            | String   | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |
