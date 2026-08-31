---
site: okex
lang: en
name: Get options open interest and volume
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get options open interest and volume

Retrieve the open interest and trading volume for options.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/option/open-interest-volume`

> Request Example

```shell
GET /api/v5/rubik/stat/option/open-interest-volume?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the open interest and trading volume for options
result = tradingDataAPI.get_options_interest_volume(
    ccy="BTC",
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- | :------------------------------------------------ |
| ccy      | String | Yes      | Currency |
| period   | String | No      | Period, the default is `8H`. e.g. [`8H/1D`]  <br>Each granularity can only query 72 pieces of data at the earliest                                |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630368000000",
            "3458.1000",
            "78.8000"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                             |
| :------------ | :------- |:------------------------------------------------------------|
| ts         | String     | Timestamp                                                   |
| oi         | String     | Total open interest , unit in `ccy` (in request parameter)  |
| vol        | String     | Total trading volume , unit in `ccy` (in request parameter) |

<aside class="notice">
The return value array order is: [ts,oi,vol]
</aside>
