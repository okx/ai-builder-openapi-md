---
site: okex
lang: en
name: GET / Order book
method: GET
path: /api/v5/market/books
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Order book

Retrieve order book of the instrument. The data will be updated once every 50 milliseconds. Best ask price may be lower than the best bid price during the pre-open period.<br>
This endpoint does not return data immediately. Instead, it returns the latest data once the server-side cache has been updated.

#### Rate Limit: 40 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/books`

> Request Example

```shell
GET /api/v5/market/books?instId=BTC-USDT

```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve order book of the instrument
result = marketDataAPI.get_orderbook(
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                           |
| :----- | :----- | :------- | :--------------------------------------------- |
| instId | String | Yes      | Instrument ID, e.g. `BTC-USDT`         |
| sz     | String | No       | Order book depth per side. Maximum 400, e.g. 400 bids + 400 asks <br>Default returns to `1` depth data |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "41006.8",
                    "0.60038921",
                    "0",
                    "1"
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "0",
                    "2"
                ]
            ],
            "ts": "1629966436396",
            "seqId": 3235851742
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**       |
| :--------- | :------- | :------------- |
| asks       | Array of Arrays    | Order book on sell side |
| bids       | Array of Arrays    | Order book on buy side |
| ts         | String   | Order book generation time |
| seqId      | Integer  | Sequence ID of the current message|

<aside class="notice">
An example of the array of asks and bids values: ["411.8", "10", "0", "4"]<br>
- "411.8" is the depth price<br>
- "10" is the quantity at the price (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)<br>
- "0" is part of a deprecated feature and it is always "0"<br>
- "4" is the number of orders at the price.<br>
</aside>

<aside class="notice">
The order book data will be updated around once a second during the call auction.
</aside>
