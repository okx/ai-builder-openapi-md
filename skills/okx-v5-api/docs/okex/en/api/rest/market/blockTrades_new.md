---
site: okex
lang: en
name: Get public single-leg transactions of block trades
method: GET
path: /api/v5/public/block-trades
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get public single-leg transactions of block trades

Retrieve the recent block trading transactions of an instrument. Descending order by tradeId. The data will be updated 15 minutes after the block trade execution.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/public/block-trades`

> Request Example

```shell
GET /api/v5/public/block-trades?instId=BTC-USDT
```

#### Request Parameters

| Parameter | Type   | Required | Description                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USDT`                      |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "fillVol": "5",
            "fwdPx": "26857.86591585",
            "groupId": "",
            "idxPx": "26889.7",
            "instId": "BTC-USD-231013-22000-P",
            "markPx": "0.0000000000000001",
            "px": "0.0026",
            "side": "buy",
            "sz": "1",
            "tradeId": "632960608383700997",
            "ts": "1697181568974"
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
| side       | String   | Trade side <br>`buy`  <br>`sell` |
| fillVol | String | Implied volatility <br>Only applicable to `OPTION` | 
| fwdPx | String | Forward price <br>Only applicable to `OPTION` | 
| idxPx | String | Index price  <br>Applicable to `FUTURES`, `SWAP`, `OPTION` | 
| markPx | String | Mark price  <br>Applicable to `FUTURES`, `SWAP`, `OPTION` |  
| groupId | String | Group RFQ ID<br> Only applicable to group RFQ, return "" for normal RFQ |
| ts         | String   | Trade time, Unix timestamp format in milliseconds, e.g. `1597026383085`. |

<aside class="notice">
Up to 500 most recent historical public transaction data can be retrieved.
</aside>

<aside class="notice">
Group RFQ introduction<br><br>

1. Add new response parameter groupId, facilitating clients to map subaccount execution to group RFQ. Only applicable to group RFQ, return "" for normal RFQ.<br>
2. Data return by this endpoint should be at **child RFQ execution level** but split into a single leg. tradeId will be populated.
</aside>
