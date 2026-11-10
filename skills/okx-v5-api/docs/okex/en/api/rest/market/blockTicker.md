---
site: okex
lang: en
name: Get block ticker
method: GET
path: /api/v5/market/block-ticker
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get block ticker

Retrieve the latest block trading volume in the last 24 hours.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/block-ticker`

> Request Example

```shell
GET /api/v5/market/block-ticker?instId=LTC-USD-SWAP
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Retrieve the latest block trading volume in the last 24 hours
result = marketDataAPI.get_block_ticker(
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                          |
| :----- | :----- | :------- | :---------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USD-SWAP` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
        "instType":"SWAP",
        "instId":"LTC-USD-SWAP",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "ts":"1597026383085"
     }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
|:--------------| :------- | :----------------------------------------------------------- |
| instId        | String   | Instrument ID                                                                                                                                                                                            |
| instType      | String   | Instrument type                                                                                                                                                                                          |
| volCcy24h     | String   | 24h trading volume, with a unit of `currency`. <br>If it is a `derivatives` contract, the value is the number of base currency. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in quote currency. |
| vol24h        | String   | 24h trading volume, with a unit of `contract`. <br>If it is a `derivatives` contract, the value is the number of contracts. <br>If it is `SPOT`/`MARGIN`, the value is the quantity in base currency.       |
| ts            | String   | Block ticker data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                           |
