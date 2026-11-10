---
site: my
lang: en
name: Get index components
method: GET
path: /api/v5/market/index-components
module: market
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get index components

Get the index component information data on the market

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/index-components`

> Request Example

```shell
GET /api/v5/market/index-components?index=BTC-USD
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Get the index component information data on the market
result = marketDataAPI.get_index_components(
    index="BTC-USD"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| index | String | Yes       | index, e.g `BTC-USDT`<br>Same as `uly`.                      |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": {
        "components": [
            {
                "symbol": "BTC/USDT",
                "symPx": "52733.2",
                "wgt": "0.25",
                "cnvPx": "52733.2",
                "exch": "OKX"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52739.87000000",
                "wgt": "0.25",
                "cnvPx": "52739.87000000",
                "exch": "Binance"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52729.1",
                "wgt": "0.25",
                "cnvPx": "52729.1",
                "exch": "Huobi"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52739.47929397",
                "wgt": "0.25",
                "cnvPx": "52739.47929397",
                "exch": "Poloniex"
            }
        ],
        "last": "52735.4123234925",
        "index": "BTC-USDT",
        "ts": "1630985335599"
    }
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| index     | String   | Index                                               |
| last      | String   | Latest Index Price                                               |
| ts         | String   | Data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`    |
| components | Array of objects   |  Components       |
| > exch         | String   | Name of Exchange                                           |
| > symbol       | String   | Name of Exchange Trading Pairs             |
| > symPx         | String   | Price of the exchange trading pair  |
| > wgt         | String   | Weights |
| > cnvPx         | String   | Component price used for index calculation. Derived from `symPx` after configured processing, which may include quote currency conversion, multiplier adjustment (e.g. ×10 or ×0.1), or EMA smoothing. May differ from `symPx`. |
