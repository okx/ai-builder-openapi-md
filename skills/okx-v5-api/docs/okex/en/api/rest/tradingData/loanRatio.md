---
site: okex
lang: en
name: Get margin long/short ratio
method: GET
path: /api/v5/rubik/stat/margin/loan-ratio
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get margin long/short ratio

Retrieve the ratio of cumulative amount of quote currency to base currency.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/margin/loan-ratio`

> Request Example

```shell
GET /api/v5/rubik/stat/margin/loan-ratio?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the ratio of cumulative amount between currency margin quote currency and base currency
result = tradingDataAPI.get_margin_lending_ratio(
    ccy="BTC",
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                                                                                                                                                              |
| :--------- | :------ | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String | Yes      | Currency                                                                                                                                                                                                                                                                                 |
| begin    | String | No      | Begin time, e.g. `1597026383085`                                                                                                                                                                                                                                                         |
| end      | String | No      | End time, e.g. `1597026383085`                                                                                                                                                                                                                                                           |
| period   | String | No      | Period<br>`m`: Minute, `H`: Hour, `D`: Day<br>the default is `5m`, e.g. [`5m`/`1H`/`1D`]  <br>`5m` granularity can only query data within two days at most<br>`1H` granularity can only query data within 30 days at most<br>`1D` granularity can only query data within 180 days at most |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630492800000",
            "0.4614"
        ],
        [
            "1630492500000",
            "0.5767"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| ts         | String     | Timestamp                                                |
| ratio      | String     | Margin lending ratio                                             |

<aside class="notice">
The return value array order is: [ts,ratio]
</aside>
