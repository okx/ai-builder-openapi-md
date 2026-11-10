---
site: okex
lang: en
name: Set Quote products
method: POST
path: /api/v5/rfq/maker-instrument-settings
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set Quote products

Customize the products which makers want to quote and receive RFQs for, and the corresponding price and size limit. 

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/maker-instrument-settings`

> Request Example

```shell
POST /api/v5/rfq/maker-instrument-settings
body
[
    {
     "instType": "OPTION",
     "data":
        [{
            "instFamily": "BTC-USD",
            "maxBlockSz": "10000",
            "makerPxBand": "5"
        },
        {
            "instFamily": "SOL-USD",
            "maxBlockSz": "100000",
            "makerPxBand": "15"
        }]
    },
    {
     "instType": "FUTURES",
     "data":
        [{
            "instFamily": "BTC-USD",
            "maxBlockSz": "10000",
            "makerPxBand": "5"
        },
        {
            "instFamily": "ETH-USDT",
            "maxBlockSz": "100000",
            "makerPxBand": "15"
        }]
    },
    {
     "instType": "SWAP",
     "data":
        [{
            "instFamily": "BTC-USD",
            "maxBlockSz": "10000",
            "makerPxBand": "5"
         },
        {
            "instFamily": "ETH-USDT"
        }]
    },
    {
    "instType": "SPOT",
     "data":
        [{
            "instId": "BTC-USDT"
         },
        {
            "instId": "TRX-USDT"
        }]
    }
]
```
```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Set quote products
data =[{
    "instType": "OPTION",
    "data": [{
            "uly": "BTC-USD",
            "maxBlockSz": "10000",
            "makerPxBand": "5"
        },
        {
            "uly": "SOL-USD",
            "maxBlockSz": "100000",
            "makerPxBand": "15"
        }
    ]
}]

result = blockTradingAPI.set_marker_instrument(
    data
)
print(result)
```

#### Request parameters
| Parameter     | Type | Required | Description                                                                                                                                                                                                         |
|:---------------| :----- |:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType       |String | Yes      | Type of instrument. Valid value can be `FUTURES`, `OPTION`, `SWAP` or `SPOT`.                                                                                                                                       |
| includeAll     |Boolean | No       | Receive all instruments or not under specific instType setting. <br>Valid value can be boolean (`True`/`False`). By default, the value will be `false`.                                                                       |
| data           |Array of objects | Yes      | Elements of the instType.                                                                                                                                                                                           |
| > instFamily   |String | Conditional | Instrument family. Required for `FUTURES`, `OPTION` and `SWAP` only.                                                                                                             |
| > instId       |String | Conditional | Instrument ID. Required for `SPOT` only.                                                                                                                                                                            |
| > maxBlockSz   |String | No       | Max trade quantity for the product(s). <br>For `FUTURES`, `OPTION` and `SWAP`, the max quantity of the RFQ/Quote is in unit of contracts. For `SPOT`, this parameter is in base currency.                                                                                    | 
| > makerPxBand  |String | No       | Price bands in unit of ticks, measured against mark price. <br>Setting makerPxBand to 1 tick means: <br>If Bid price > Mark + 1 tick, it will be stopped <br>If Ask price < Mark - 1 tick, It will be stopped |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "result":true
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                              |
| :------ | :----- | :-----------------------------------------------------------  |
| code	            | String|   The result code, `0` means success.                |
| msg	            | String|   The error message, not empty if the code is not `0`. |
| data	            | Array of objects|   Array of objects containing the results. |
| > result            | Boolean|  Result of the request<br>Valid value is `true` or `false`. |
