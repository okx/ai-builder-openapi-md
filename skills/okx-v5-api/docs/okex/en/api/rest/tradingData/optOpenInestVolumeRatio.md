---
site: okex
lang: en
name: Get put/call ratio
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume-ratio
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get put/call ratio

Retrieve the open interest ratio and trading volume ratio of calls vs puts.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/option/open-interest-volume-ratio`

> Request Example

```shell
GET /api/v5/rubik/stat/option/open-interest-volume-ratio?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the open interest ratio and trading volume ratio of calls vs puts
result = tradingDataAPI.get_put_call_ratio(
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
            "1630512000000",
            "2.7261",
            "2.3447"
        ],
        [
            "1630425600000",
            "2.8101",
            "2.3438"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                   |
| :------------ | :------- |:----------------------------------|
| ts         | String     | Timestamp of data generation time |
| oiRatio         | String     | Long/Short open interest ratio    |
| volRatio        | String     | Long/Short trading volume ratio   |

<aside class="notice">
The return value array order is: [ts,oiRatio,volRatio]
</aside>
