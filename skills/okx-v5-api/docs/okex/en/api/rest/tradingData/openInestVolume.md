---
site: okex
lang: en
name: Get contracts open interest and volume
method: GET
path: /api/v5/rubik/stat/contracts/open-interest-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get contracts open interest and volume

Retrieve the open interest and trading volume for Expiry Futures and Perpetual Futures.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/contracts/open-interest-volume`

> Request Example

```shell
GET /api/v5/rubik/stat/contracts/open-interest-volume?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the open interest and trading volume for Expiry Futures and Perpetual Futures
result = tradingDataAPI.get_contracts_interest_volume(
    ccy="BTC",
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- | :------------------------------------------------ |
| ccy      | String | Yes      | Currency |
| begin    | String | No      | Begin time, e.g. `1597026383085`                                          |
| end      | String | No      | End time, e.g. `1597026383011`                                        |
| period   | String | No      | Period, the default is `5m`, e.g. [`5m/1H/1D`]  <br>`5m` granularity can only query data within two days at most<br>`1H` granularity can only query data within 30 days at most <br>`1D` granularity can only query data within 180 days at most                               |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630502400000",
            "1713028741.6898",
            "39800873.554"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**           |
| :------------ | :------- |:--------------------------|
| ts         | String     | Timestamp                 |
| oi         | String     | Total open interest（USD）  |
| vol        | String     | Total trading volume（USD） |

<aside class="notice">
The return value array order is: [ts,oi,vol]
</aside>
