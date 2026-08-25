---
site: turkey
lang: en
name: GET / Trades history
method: GET
path: /api/v5/market/history-trades
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Trades history

Retrieve the recent transactions of an instrument from the last 3 months with pagination.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/history-trades`

> Request Example

```shell
GET /api/v5/market/history-trades?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve the recent transactions of an instrument from the last 3 months with pagination
result = marketDataAPI.get_history_trades(
    instId="BTC-USD-SWAP"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                 |
| :----- | :----- |:---------|:--------------------------------------------------------------------------------------------|
| instId | String | Yes      | Instrument ID, e.g. `BTC-USDT`                                                              |
| type | String | No       | Pagination Type <br>`1`: tradeId `2`: timestamp<br>The default is `1`           |
| after | String | No       | Pagination of data to return records earlier than the requested tradeId or ts. |
| before | String | No       | Pagination of data to return records newer than the requested tradeId. <br>Do not support timestamp for pagination. The latest data will be returned when using `before` individually  |
| limit  | String | No       | Number of results per request. The maximum and default both are `100` |

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
| source | String | Order source<br> `0`: normal order<br>`1`: Enhanced Liquidity Program order |
| ts         | String   | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |
