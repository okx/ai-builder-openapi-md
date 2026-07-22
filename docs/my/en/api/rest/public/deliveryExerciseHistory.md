---
site: my
lang: en
name: Get delivery/exercise history
method: GET
path: /api/v5/public/delivery-exercise-history
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

###  Get delivery/exercise history

Retrieve delivery records of Futures and exercise records of Options in the last 3 months.

#### Rate Limit: 40 requests per 2 seconds
#### Rate limit rule: IP + (Instrument Type + instFamily)

#### HTTP Request

`GET /api/v5/public/delivery-exercise-history`

> Request Example

```shell
GET /api/v5/public/delivery-exercise-history?instType=OPTION&instFamily=BTC-USD
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve delivery records of Futures and exercise records of Options in the last 3 months
result = publicDataAPI.get_delivery_exercise_history(
    instType="FUTURES",
    uly="BTC-USD"
)
print(result)
```

#### Request Parameters

| Parameter        | Type   | Required      | Description                                                                                                                               |
|:-----------------| :----- |:--------------|:------------------------------------------------------------------------------------------------------------------------------------------|
| instType         | String | Yes           | Instrument type<br>`FUTURES`<br>`OPTION`                                                                                               |
| instFamily       | String | Yes           | Instrument family, only applicable to `FUTURES`/`OPTION`                                                                                   |
| after            | String | No            | Pagination of data to return records earlier than the requested `ts`                                                                      |
| before           | String | No            | Pagination of data to return records newer than the requested `ts`                                                                        |
| limit            | String | No            | Number of results per request. The maximum is `100`; The default is `100`                                                                 |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ts":"1597026383085",
            "details":[
                {
                    "type":"delivery",
                    "insId":"BTC-USD-190927",
                    "px":"0.016"
                }
            ]
        },
        {
            "ts":"1597026383085",
            "details":[
                {
                    "insId":"BTC-USD-200529-6000-C",
                    "type":"exercised",
                    "px":"0.016"
                },
                {
                    "insId":"BTC-USD-200529-8000-C",
                    "type":"exercised",
                    "px":"0.016"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                                                         |
| :----- |:-------| :----------------------------------------------------------- |
| ts     | String | Delivery/exercise time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| details  | Array of objects  | Delivery/exercise details            |
| > insId  | String | Delivery/exercise contract ID     |
| > px     | String | Delivery/exercise price           |
| > type   | String | Type  <br>`delivery` <br>`exercised` <br>`expired_otm`:Out of the money |
