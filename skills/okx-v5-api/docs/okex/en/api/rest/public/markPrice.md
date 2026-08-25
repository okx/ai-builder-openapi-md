---
site: okex
lang: en
name: Get mark price
method: GET
path: /api/v5/public/mark-price
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get mark price

Retrieve mark price.

We set the mark price based on the SPOT index and at a reasonable basis to prevent individual users from manipulating the market and causing the contract price to fluctuate.

#### Rate Limit: 10 requests per 2 seconds 
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/mark-price`

> Request Example

```shell
GET /api/v5/public/mark-price?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve mark price
result = publicDataAPI.get_mark_price(
    instType="SWAP",
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- | :------- | :----------------------------------------------------------- |
| instType   | String | Yes       | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instFamily | String | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`   |                                                                                                                                                                      
| instId     | String | No       | Instrument ID, e.g. `BTC-USD-SWAP`                                |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "markPx":"200",
        "ts":"1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType   | String   | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instId      | String   | Instrument ID, e.g. `BTC-USD-200214`                              |
| markPx    | String   | Mark price                                           |
| ts         | String   | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
