---
site: okex
lang: en
name: Get open interest and volume (expiry)
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume-expiry
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get open interest and volume (expiry)

Retrieve the open interest and trading volume of calls and puts for each upcoming expiration.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/option/open-interest-volume-expiry`

> Request Example

```shell
GET /api/v5/rubik/stat/option/open-interest-volume-expiry?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the open interest and trading volume of calls and puts for each upcoming expiration
result = tradingDataAPI.get_interest_volume_expiry(
    ccy="BTC"
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- | :------------------------------------------------ |
| ccy      | String | Yes      | Currency |
| period   | String | No      | Period, the default is `8H`. e.g. [`8H/1D`]  <br>Each granularity can provide only one latest piece of data                                |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630540800000",
            "20210902",
            "6.4",
            "18.4",
            "0.7",
            "0.4"
        ],
        [
            "1630540800000",
            "20210903",
            "47",
            "36.6",
            "1",
            "10.7"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| ts         | String     | Timestamp                                                |
| expTime    | String     | Contract expiry date, the format is `YYYYMMDD`, e.g. `20210623`                            |
| callOI     | String     | Total call open interest (`coin` as the unit)                                        |
| putOI      | String     | Total put  open interest (`coin` as the unit)                                    |
| callVol    | String     | Total call trading volume (`coin` as the unit)                                   |
| putVol     | String     | Total put trading volume (`coin` as the unit)                                     |

<aside class="notice">
The return value array order is: [ts,expTime,callOI,putOI,callVol,putVol]
</aside>
