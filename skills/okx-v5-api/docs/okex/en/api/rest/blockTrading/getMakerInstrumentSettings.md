---
site: okex
lang: en
name: Get Quote products
method: GET
path: /api/v5/rfq/maker-instrument-settings
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get Quote products

Retrieve the products which makers want to quote and receive RFQs for, and the corresponding price and size limit. 

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/rfq/maker-instrument-settings`

> Request Example

```shell
GET /api/v5/rfq/maker-instrument-settings
```

#### Request parameters
None

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "OPTION",
            "includeAll": true,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "SOL-USD",
                    "maxBlockSz": "100000",
                    "makerPxBand": "15"
                }
            ]
        },
        {
            "instType": "FUTURES",
            "includeAll": false,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "ETH-USDT",
                    "maxBlockSz": "100000",
                    "makerPxBand": "15"
                }
            ]
        },
        {
            "instType:": "SWAP",
            "includeAll": false,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "ETH-USDT"
                }
            ]
        },
        {
            "instType:": "SPOT",
            "includeAll": false,
            "data": [
                {
                    "instId": "BTC-USDT"
                },
                {
                    "instId": "TRX-USDT"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                              |
| :------ | :----- | :-----------------------------------------------------------  |
| code	         | String | The result code, `0` means success.                |
| msg	         | String | The error message, not empty if the code is not `0`. |
| data	         | Array of objects | Return data of the request. |
| > instType       | String | Type of instrument. Valid value can be `FUTURES`, `OPTION`, `SWAP` or `SPOT`.                                                                                                                                       |
| > includeAll     | Boolean| Receive all instruments or not under specific instType setting. <br>Valid value can be boolean (`True`/`False`). By default, the value will be `false`.                                                                       |
| > data           | Array of objects | Elements of the instType.                                                                                                                                                                                           |
| >> instFamily   | String | Instrument family. Required for `FUTURES`, `OPTION` and `SWAP` only.                                                                                                             |
| >> instId       | String | Instrument ID. Required for `SPOT` only.                                                                                                                                                                            |
| >> maxBlockSz   | String | Max trade quantity for the product(s). <br>For `FUTURES`, `OPTION` and `SWAP`, the max quantity of the RFQ/Quote is in unit of contracts. For `SPOT`, this parameter is in base currency.                                                                   | 
| >> makerPxBand  | String | Price bands in unit of ticks, measured against mark price. <br>Setting makerPxBand to 1 tick means: <br>If Bid price > Mark + 1 tick, it will be stopped <br>If Ask price < Mark - 1 tick, It will be stopped |
