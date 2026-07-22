---
site: us
lang: en
name: GET / Option trades by instrument family
method: GET
path: /api/v5/market/option/instrument-family-trades
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / Option trades by instrument family

Retrieve the recent transactions of an instrument under same instFamily. The maximum is 100.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/option/instrument-family-trades`

> Request Example

```shell
GET /api/v5/market/option/instrument-family-trades?instFamily=BTC-USD
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                               |
| :----- | :----- | :------- |:--------------------------------------------------------------------------|
| instFamily | String | Yes       | Instrument family, e.g. BTC-USD<br>Applicable to `OPTION`                       |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "vol24h": "103381",
            "tradeInfo": [
                {
                    "instId": "BTC-USD-221111-17750-C",
                    "side": "sell",
                    "sz": "1",
                    "px": "0.0075",
                    "tradeId": "20",
                    "ts": "1668090715058"
                },
                {
                    "instId": "BTC-USD-221111-17750-C",
                    "side": "sell",
                    "sz": "91",
                    "px": "0.01",
                    "tradeId": "19",
                    "ts": "1668090421062"
                }
            ],
            "optType": "C"
        },
        {
            "vol24h": "144499",
            "tradeInfo": [
                {
                    "instId": "BTC-USD-230127-10000-P",
                    "side": "sell",
                    "sz": "82",
                    "px": "0.019",
                    "tradeId": "23",
                    "ts": "1668090967057"
                },
                {
                    "instId": "BTC-USD-221111-16250-P",
                    "side": "sell",
                    "sz": "102",
                    "px": "0.0045",
                    "tradeId": "24",
                    "ts": "1668090885050"
                }
            ],
            "optType": "P"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                        |
| :--------- | :------- |:-----------------------------------------------------------------------|
| vol24h     | String   | 24h trading volume, with a unit of contract.                           |
| optType    | String   | Option type, C: Call P: Put                                            |
| tradeInfo         | Array of objects   | The list trade data                                                    |
| > instId         | String   | The Instrument ID                                                      |
| > tradeId       | String   | Trade ID                                                               |
| > px         | String   | Trade price                                                            |
| > sz         | String   | Trade quantity. The unit is contract.                                  |
| > side         | String   | Trade side<br>`buy`<br>`sell`                                          |
| > ts         | String   | Trade time, Unix timestamp format in milliseconds, e.g. 1597026383085. |
