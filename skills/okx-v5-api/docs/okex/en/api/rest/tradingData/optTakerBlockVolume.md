---
site: okex
lang: en
name: Get taker flow
method: GET
path: /api/v5/rubik/stat/option/taker-block-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get taker flow

This shows the relative buy/sell volume for calls and puts. It shows whether traders are bullish or bearish on price and volatility.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/rubik/stat/option/taker-block-volume`

> Request Example

```shell
GET /api/v5/rubik/stat/option/taker-block-volume?ccy=BTC
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # Production trading:0 , demo trading:1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# This shows the relative buy/sell volume for calls and puts. It shows whether traders are bullish or bearish on price and volatility
result = tradingDataAPI.get_taker_block_volume(
    ccy="BTC",
)
print(result)
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                    |
| :--------- | :------ | :------- | :------------------------------------------------ |
| ccy      | String | Yes      | currency |
| period   | String | No      | period, the default is `8H`. e.g. [`8H/1D`]  <br>Each granularity can provide only one latest piece of data    |

> Response Example

```json
{
    "code": "0",
    "data": [
        "1630512000000",
        "8.55",
        "67.3",
        "16.05",
        "16.3",
        "126.4",
        "40.7"
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| ts                 | String     | Timestamp                                                |
| callBuyVol         | String     | call option buy volume,  in settlement currency                                    |
| callSellVol        | String     | call option sell volume,  in settlement currency                                          |
| putBuyVol          | String     | put option buy volume,  in settlement currency                                    |
| putSellVol         | String     | put option sell volume,  in settlement currency                                          |
| callBlockVol       | String     | call block volume                                       |
| putBlockVol        | String     | put  block volume                                            |

<aside class="notice">
The return value array order is: [ts,callBuyVol,callSellVol,putBuyVol,putSellVol,callBlockVol,putBlockVol]
</aside>
