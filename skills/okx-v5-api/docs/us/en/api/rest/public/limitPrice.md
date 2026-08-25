---
site: us
lang: en
name: Get limit price
method: GET
path: /api/v5/public/price-limit
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Get limit price

Retrieve the highest buy limit and lowest sell limit of the instrument.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/price-limit`

> Request Example

```shell
GET /api/v5/public/price-limit?instId=BTC-USDT-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve the highest buy limit and lowest sell limit of the instrument
result = publicDataAPI.get_price_limit(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                      |
| :----- | :----- | :------- | :-------------------------------------------------------- |
| instId | String | Yes | Instrument ID, e.g. `BTC-USDT-SWAP` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "buyLmt":"17057.9",
        "sellLmt":"16388.9",
        "ts":"1597026383085",
        "enabled": true
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType   | String   | Instrument type |
| instId      | String   | Instrument ID, e.g. `BTC-USDT-SWAP`                                |
| buyLmt | String | Highest buy limit <br>Return "" when enabled is false |
| sellLmt | String | Lowest sell limit <br>Return "" when enabled is false |
| ts | String | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| enabled | Boolean | Whether price limit is effective <br>`true`: the price limit is effective <br>`false`: the price limit is not effective |
