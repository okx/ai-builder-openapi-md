---
site: okex
lang: en
name: Get open interest and volume (strike)
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume-strike
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get open interest and volume (strike)

Retrieve the taker volume for both buyers and sellers of calls and puts.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/option/open-interest-volume-strike`

> Request Example

```shell
GET /api/v5/rubik/stat/option/open-interest-volume-strike?ccy=BTC&expTime=20210901
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the taker volume for both buyers and sellers of calls and puts
result = tradingDataAPI.get_interest_volume_strike(
    ccy="BTC",
    expTime="20210623"
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- | :------------------------------------------------ |
| ccy      | String | Yes      | Currency |
| expTime  | String | Yes      | Contract expiry date, the format is `YYYYMMdd`, e.g. `20210623`|
| period   | String | No      | Period, the default is `8H`. e.g. [`8H/1D`]  <br>Each granularity can provide only one latest piece of data                                |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630540800000",
            "10000",
            "0",
            "0.5",
            "0",
            "0"
        ],
        [
            "1630540800000",
            "14000",
            "0",
            "5.2",
            "0",
            "0"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| ts         | String     | Timestamp                                                |
| strike     | String     | Strike price                                         |
| callOI     | String     | Total call open interest (`coin` as the unit)                                     |
| putOI      | String     | Total put open interest (`coin` as the unit)                                   |
| callVol    | String     | Total call trading volume (`coin` as the unit)                                    |
| putVol     | String     | Total put trading volume (`coin` as the unit)                           |

<aside class="notice">
The return value array order is: [ts,strike,callOI,putOI,callVol,putVol]
</aside>
