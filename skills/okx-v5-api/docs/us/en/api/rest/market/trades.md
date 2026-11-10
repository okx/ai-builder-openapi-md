---
site: us
lang: en
name: GET / Trades
method: GET
path: /api/v5/market/trades
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### GET / Trades

Retrieve the recent transactions of an instrument.

#### Rate Limit: 100 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/trades`

> Request Example

```shell
GET /api/v5/market/trades?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve the recent transactions of an instrument
result = marketDataAPI.get_trades(
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USDT`                      |
| limit  | String | No       | Number of results per request. The maximum is `500`; The default is `100` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "source": "0",
            "px": "29963.2",
            "tradeId": "242720720",
            "ts": "1654161646974"
        },
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "source": "0",
            "px": "29964.1",
            "tradeId": "242720719",
            "ts": "1654161641568"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| instId     | String   | Instrument ID                    |
| tradeId    | String   | Trade ID                                       |
| px         | String   | Trade price                                 |
| sz         | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| side       | String   | Trade side of taker <br>`buy`  <br>`sell` |
| source | String | Order source<br> `0`: normal order<br> `1`: RPI order |
| ts         | String   | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |

<aside class="notice">
Up to 500 most recent historical public transaction data can be retrieved.
</aside>
