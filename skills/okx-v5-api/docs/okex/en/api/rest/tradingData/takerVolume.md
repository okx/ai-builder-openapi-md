---
site: okex
lang: en
name: Get taker volume
method: GET
path: /api/v5/rubik/stat/taker-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get taker volume

Retrieve the taker volume for both buyers and sellers.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/taker-volume`

> Request Example

```shell
GET /api/v5/rubik/stat/taker-volume?ccy=BTC&instType=SPOT
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# Retrieve the taker volume for both buyers and sellers
result = tradingDataAPI.get_taker_volume(
    ccy="BTC",
    instType="SPOT"
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required  | Description                                                                                                                                                                                                                                           |
| :--------- | :------ |:----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String | Yes       | Currency                                                                                                                                                                                                                                              |
| instType | String | Yes       | Instrument type<br>`SPOT`<br>`CONTRACTS`                                                                                                                                                                                                              |
| begin    | String | No        | Begin time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                               |
| end      | String | No        | End time, Unix timestamp format in milliseconds, e.g. `1597026383011`                                                                                                                                                                                 |
| period   | String | No        | Period, the default is `5m`, e.g. [`5m`/`1H`/`1D`]  <br>`5m` granularity can only query data within two days at most<br>`1H` granularity can only query data within 30 days at most <br>`1D` granularity can only query data within 180 days at most |

> Response Example

```json
{
    "code": "0",
    "data": [
        [
            "1630425600000",
            "7596.2651",
            "7149.4855"
        ],
        [
            "1630339200000",
            "5312.7876",
            "7002.7541"
        ]
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| ts            | String   | Timestamp                                   |
| sellVol       | String   | Sell volume                               |
| buyVol        | String   | Buy volume                                 |

<aside class="notice">
The return value array order is: [ts,sellVol,buyVol]
</aside>
