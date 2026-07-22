---
site: us
lang: en
name: Get estimated delivery/exercise price
method: GET
path: /api/v5/public/estimated-price
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Get estimated delivery/exercise price

Retrieve the estimated delivery price which will only have a return value one hour before the delivery/exercise.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/estimated-price`

> Request Example

```shell
GET /api/v5/public/estimated-price?instId=BTC-USD-200214
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve estimated delivery/exercise price
result = publicDataAPI.get_estimated_price(
    instId = "BTC-USD-200214",
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                     |
| :----- | :----- | :------- |:--------------------------------------------------------------------------------|
| instId | String | Yes       | Instrument ID, e.g. `BTC-USD-200214` <br>only applicable to `FUTURES`/`OPTION`/`EVENTS` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"FUTURES",
        "instId":"BTC-USDT-201227",
        "settlePx":"200",
        "ts":"1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                 |
| :--------- | :------- |:--------------------------------------------------------------------------------|
| instType   | String   | Instrument type<br>`FUTURES`<br>`OPTION`                                        |
| instId     | String   | Instrument ID, e.g. `BTC-USD-200214`                                            |
| settlePx   | String   | Estimated delivery/exercise price                                               |
| ts         | String   | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
