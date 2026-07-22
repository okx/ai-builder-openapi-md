---
site: okex
lang: en
name: GET / RSI back testing (public)
method: GET
path: /api/v5/tradingBot/public/rsi-back-testing
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / RSI back testing (public)

Authentication is not required for this public endpoint.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/tradingBot/public/rsi-back-testing`

> Request Example

```shell
GET /api/v5/tradingBot/public/rsi-back-testing?instId=BTC-USDT&thold=30&timeframe=3m&timePeriod=14
```

#### Request Parameters

| Parameter   | Type      | Required   | Description                                                                                                                                                    |
|:------------|:----------|:-----------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId      | String    | Yes        | Instrument ID, e.g. `BTC-USDT`<br>Only applicable to `SPOT`                                                                                                    |
| timeframe   | String    | Yes        | K-line type<br>`3m`, `5m`, `15m`, `30m` (`m`: minute)<br>`1H`, `4H` (`H`: hour)<br>`1D` (`D`: day) |
| thold       | String    | Yes        | Threshold<br>The value should be an integer between 1 to 100                                                                                                   |
| timePeriod  | String    | Yes        | Time Period<br>`14`                                                                                                                                            |
| triggerCond | String    | No         | Trigger condition<br>`cross_up`<br>`cross_down`<br>`above`<br>`below`<br>`cross`<br>Default is `cross_down`                                                    |
| duration    | String    | No         | Back testing duration<br>`1M` (`M`: month)<br>Default is `1M`                                                                                                      |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "triggerNum": "164"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                    |
|:----------------| :------- |:-----------------------------------|
| triggerNum      | String   | Trigger number |
