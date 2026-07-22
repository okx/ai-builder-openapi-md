---
site: hongkong
lang: en
name: Economic calendar channel
type: guide
---

### Economic calendar channel

<aside class="notice">
This endpoint is only supported in production environment.
</aside>

Retrieve the most up-to-date economic calendar data. This endpoint is only applicable to VIP 1 and above users in the trading fee tier.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
      {
          "channel": "economic-calendar"
      }
    ]
}

```

```python
import asyncio
from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
      {
          "channel": "economic-calendar"
      }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request parameters

| Parameter | Type             | Required | Description                               |
|-----------|------------------| --- |-------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String           | Yes | Operation<br>`subscribe`<br>`unsubscribe` |
| args      | Array of objects | Yes | List of subscribed channels               |
| > channel | String           | Yes | Channel name<br>`economic-calendar`       |

> Successful Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "economic-calendar"
    },
    "connId": "a4d3ae55"
}

```

> Failure Response Example

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"economic-calendar\", \"instId\" : \"LTC-USD-190628\"}]}",
  "connId": "a4d3ae55"
}

```

#### Response parameters

| Parameter | Type     | Required | Description                                      |
|-----------|----------|----------|--------------------------------------------------|
| id           | String   | No       | Unique identifier of the message                        |
| event     | String   | Yes      | Event<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | No       | Subscribed channel                               |
| > channel | String   | Yes      | Channel name                                     |
| code      | String   | No       | Error code                                       |
| msg       | String   | No       | Error message                                    |
| connId    | String   | Yes      | WebSocket connection ID                          |

> Push Data Example

```json
{
    "arg": {
        "channel": "economic-calendar"
    },
    "data": [
        {
            "calendarId": "319275",
            "date": "1597026383085",
            "region": "United States",
            "category": "Manufacturing PMI",
            "event": "S&P Global Manufacturing PMI Final",
            "refDate": "1597026383085",
            "actual": "49.2",
            "previous": "47.3",
            "forecast": "49.3",
            "importance": "2",
            "prevInitial": "",
            "ccy": "",
            "unit": "",
            "ts": "1698648096590"
        }
    ]
}
```

#### Push data parameters

| Parameter     | Type             | Description                                                                                                    |
|---------------|------------------|----------------------------------------------------------------------------------------------------------------|
| arg           | Object           | Successfully subscribed channel                                                                                |
| > channel     | String           | Channel name<br>`economic-calendar`                                                                                                   |
| data          | Array of objects | Subscribed data                                                                                                |
| > event       | string           | Event name                                                                                                     |
| > region      | string           | Country, region or entity                                                                                      |
| > category    | string           | Category name                                                                                                  |
| > actual      | string           | The actual value of this event                                                                                 |
| > previous    | string           | Latest actual value of the previous period <br>The value will be revised if revision is applicable             |
| > forecast    | string           | Average forecast among a representative group of economists                                                    |
| > prevInitial | string           | The initial value of the previous period <br>Only applicable when revision happens                             |
| > date        | string           | Estimated release time of the value of actual field, millisecond format of Unix timestamp, e.g. `1597026383085` |
| > refDate     | string           | Date for which the datapoint refers to                                                                         |
| > calendarId  | string           | Calendar ID                                                                                                    |
| > unit        | string           | Unit of the data                                                                                               |
| > ccy         | string           | Currency of the data                                                                                           |
| > importance  | string           | Level of importance<br>`1`: low <br>`2`: medium <br>`3`: high                                                  |
| > ts          | string           | The time of the latest update                                                                                  |
