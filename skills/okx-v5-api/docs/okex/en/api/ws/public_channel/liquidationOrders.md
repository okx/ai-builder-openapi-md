---
site: okex
lang: en
name: Liquidation orders channel
type: guide
---

### Liquidation orders channel

Retrieve the recent liquidation orders. This data doesn’t represent the total number of liquidations on OKX.

#### URL Path

/ws/v5/public

> Request Example

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "liquidation-orders",
      "instType": "SWAP"
    }
  ]
}
```
```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "liquidation-orders",
          "instType": "SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### Request Parameters

| Parameter  | Type             | Required        | Description                                                    |
|:-----------|:-----------------|:----------------|:---------------------------------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op         | String           | Yes | Operation<br>`subscribe`<br>`unsubscribe`                      |
| args       | Array of objects | Yes | List of subscribed channels                                    |
| > channel  | String           | Yes | Channel name<br>`liquidation-orders`                           |
| > instType | String           | Yes | Instrument type<br>`SWAP`<br>`FUTURES`<br>`MARGIN`<br>`OPTION` |

> Response Example

```json
{
    "id": "1512",
    "arg": {
        "channel": "liquidation-orders",
        "instType": "SWAP"
    },
    "data": [
        {
            "details": [
                {
                    "bkLoss": "0",
                    "bkPx": "0.007831",
                    "ccy": "",
                    "posSide": "short",
                    "side": "buy",
                    "sz": "13",
                    "ts": "1692266434010"
                }
            ],
            "instFamily": "IOST-USDT",
            "instId": "IOST-USDT-SWAP",
            "instType": "SWAP",
            "uly": "IOST-USDT"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type**         | **Description**                                                                                                                                                      |
|:--------------|:-----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id           | String            | Unique identifier of the message                        |
| arg           | Object           | Successfully subscribed channel                                                                                                                                      |
| > channel     | String           | Channel name                                                                                                                                                         |
| > instId      | String           | Instrument ID                                                                                                                                                        |
| data          | Array of objects | Subscribed data                                                                                                                                                      |
| > instType    | String           | Instrument type                                                                                                                                                      |
| > instId      | String           | Instrument ID, e.g. `BTC-USD-SWAP`                                                                                                                                   |
| > uly         | String           | Underlying<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                |
| > details     | Array of objects | Liquidation details                                                                                                                                                  |
| >> side       | String           | Order side<br>`buy`<br>`sell`<br>Applicable to `FUTURES`/`SWAP`                                                                                                      |
| >> posSide    | String           | Position mode side<br>`long`: Hedge mode long<br>`short`: Hedge mode short<br>`net`: Net mode                                                                        |
| >> bkPx       | String           | Liquidation mark price. The price of the transaction with the system's liquidation account, only applicable to `FUTURES`/`SWAP`                                            |
| >> sz         | String           | Quantity of liquidation, only applicable to `MARGIN`/`FUTURES`/`SWAP`.<br/> For `MARGIN`, the unit is base currency. <br/> For `FUTURES/SWAP`, the unit is contract. |
| >> bkLoss     | String           | Bankruptcy loss                                                                                                                                                      |
| >> ccy        | String           | Liquidation currency, only applicable to  `MARGIN`                                                                                                                   |
| >> ts         | String           | Liquidation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                        |

<aside class="notice">

Liquidation data comes from different data sources, so the updated data is not necessarily in chronological order.

</aside>
