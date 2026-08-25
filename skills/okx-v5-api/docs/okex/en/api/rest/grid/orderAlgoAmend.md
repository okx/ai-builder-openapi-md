---
site: okex
lang: en
name: POST / Amend grid algo order
method: POST
path: /api/v5/tradingBot/grid/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend grid algo order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/amend-order-algo`

> Request Example

```shell
POST /api/v5/tradingBot/grid/amend-order-algo
body
{
    "algoId":"448965992920907776",
    "instId":"BTC-USDT-SWAP",
    "slTriggerPx":"1200",
    "tpTriggerPx":""
}

POST /api/v5/tradingBot/grid/amend-order-algo
body 
{
   "algoId":"578963447615062016",
   "instId":"BTC-USDT",
   "triggerParams":[
       {
           "triggerAction":"stop",  
           "triggerStrategy":"price",   
           "triggerPx":"1000"
       }
   ]
}

POST /api/v5/tradingBot/grid/amend-order-algo
body 
{
   "algoId":"578963447615062016",
   "instId":"BTC-USDT-SWAP",
   "triggerParams":[
       {
           "triggerAction":"stop",  
           "triggerStrategy":"instant",   
           "stopType":"1"
       }
   ]
}
```

#### Request Parameters

| Parameter         | Type            | Required | Description                                                                                                                                                                                            |
|:------------------|:----------------| :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| algoId            | String          | Yes       | Algo ID                                                                                                                                                                                                |
| instId            | String          | Yes       | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                    |
| slTriggerPx       | String          | No       | New stop-loss trigger price<br>if slTriggerPx is set "" means stop-loss trigger price is canceled.<br>Either `slTriggerPx` or `tpTriggerPx` is required.                                               |
| tpTriggerPx       | String          | No       | New take-profit trigger price<br>if tpTriggerPx is set "" means take-profit trigger price is canceled.                                                                                                 |
| tpRatio   | String | No       | Take profit ratio, 0.1 represents 10%, only applicable to contract grid<br>if it is set "" means take-profit ratio is canceled.                                                                        |
| slRatio   | String | No       | Stop loss ratio, 0.1 represents 10%, only applicable to contract grid`<br>if it is set "" means stop-loss ratio is canceled.                                                                          |
| topUpAmt   | String | No       | Top up amount, only applicable to spot grid                                                                          |
| triggerParams     | Array of objects | No       | Trigger Parameters                                                                                                                                                                                     |
| > triggerAction   | String          | Yes      | Trigger action<br>`start`<br>`stop`                                                                                                                                                                    |
| > triggerStrategy | String          | Yes       | Trigger strategy<br>`instant`<br>`price`<br>`rsi`                                                                                                                                                      |
| > triggerPx       | String          | No       | Trigger Price<br>This field is only valid when `triggerStrategy` is `price`                                                                                                                            |
| > stopType        | String          | No       | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions<br>This field is only valid when `triggerAction` is `stop` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId":"448965992920907776",
            "sCode":"0",
            "sMsg":"",
            "tag": ""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| algoId        | String   | Algo ID                                                    |
| algoClOrdId        | String   | Client-supplied Algo ID                                                    |
| sCode         | String   | The code of the event execution result, `0` means success. |
| sMsg          | String   | Rejection message if the request is unsuccessful.              |
| tag           | String   | Order tag                                                  |
