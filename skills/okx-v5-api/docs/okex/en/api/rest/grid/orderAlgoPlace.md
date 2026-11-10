---
site: okex
lang: en
name: POST / Place grid algo order
method: POST
path: /api/v5/tradingBot/grid/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Place grid algo order

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/order-algo`

> Request Example

```shell
# Place spot grid algo order
POST /api/v5/tradingBot/grid/order-algo
body
{
	"instId": "BTC-USDT",
	"algoOrdType": "grid",
	"maxPx": "5000",
	"minPx": "400",
	"gridNum": "10",
	"runType": "1",
	"quoteSz": "25",
	"triggerParams":[
      {
         "triggerAction":"stop",
         "triggerStrategy":"price",  
         "triggerPx":"1000"
      }
    ]
}

# Place contract grid algo order
POST /api/v5/tradingBot/grid/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"algoOrdType": "contract_grid",
	"maxPx": "5000",
	"minPx": "400",
	"gridNum": "10",
	"runType": "1",
	"sz": "200", 
	"direction": "long",
	"lever": "2",
	"triggerParams":[
      {
         "triggerAction":"start", 
         "triggerStrategy":"rsi", 
         "timeframe":"30m",
         "thold":"10",
         "triggerCond":"cross",
         "timePeriod":"14"
      },
      {
         "triggerAction":"stop",
         "triggerStrategy":"price",
         "triggerPx":"1000",
         "stopType":"2"
      }
   ]
}

```

#### Request Parameters

| Parameter                  | Type              | Required | Description                                                                                                                                                                                            |
|:---------------------------|:------------------|:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                     | String            | Yes      | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                    |
| algoOrdType                | String            | Yes      | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid                                                                                                       |
| maxPx                      | String            | Yes      | Upper price of price range                                                                                                                                                                             |
| minPx                      | String            | Yes      | Lower price of price range                                                                                                                                                                             |
| gridNum                    | String            | Yes      | Grid quantity                                                                                                                                                                                          |
| runType                    | String            | No       | Grid type<br>`1`: Arithmetic, `2`: Geometric<br>Default is Arithmetic                                                                                                  |
| tpTriggerPx                | String            | No       | TP tigger price<br>Applicable to `Spot grid`/`Contract grid`                                                                                                                                           |
| slTriggerPx                | String            | No       | SL tigger price<br>Applicable to `Spot grid`/`Contract grid`                                                                                                                                           |
| algoClOrdId                | String            | No       | Client-supplied Algo ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                          |
| tag                        | String            | No       | Order tag                                                                                                                                                                                              |
| profitSharingRatio   | String | No       | Profit sharing ratio, it only supports these values<br/>`0`,`0.1`,`0.2`,`0.3`<br/> 0.1 represents 10% |
| triggerParams              | Array of objects   | No       | Trigger Parameters<br>Applicable to `Spot grid`/`Contract grid`                                                                                                                                        |
| > triggerAction            | String            | Yes      | Trigger action<br>`start`<br>`stop`                                                                                                                                                                    |
| > triggerStrategy          | String            | Yes       | Trigger strategy<br>`instant`<br>`price`<br>`rsi`<br>Default is `instant`                                                                                                                                       |
| > delaySeconds             | String            | No       | Delay seconds after action triggered                                                                                                                                                                   |
| > timeframe                | String            | No       | K-line type<br>`3m`, `5m`, `15m`, `30m` (`m`: minute)<br>`1H`, `4H` (`H`: hour)<br>`1D` (`D`: day)<br>This field is only valid when `triggerStrategy` is `rsi`                                         |
| > thold                    | String            | No       | Threshold<br>The value should be an integer between 1 to 100<br>This field is only valid when `triggerStrategy` is `rsi`                                                                               |
| > triggerCond              | String            | No       | Trigger condition<br>`cross_up`<br>`cross_down`<br>`above`<br>`below`<br>`cross`<br>This field is only valid when `triggerStrategy` is `rsi`                                                           |
| > timePeriod               | String            | No       | Time Period<br>`14`<br>This field is only valid when `triggerStrategy` is `rsi`                                                                                                                        |
| > triggerPx                | String            | No       | Trigger Price<br>This field is only valid when `triggerStrategy` is `price`                                                                                                                            |
| > stopType                 | String            | No       | Stop type<br>Spot grid `1`: Sell base currency `2`: Keep base currency<br>Contract grid `1`: Market Close All positions `2`: Keep positions<br>This field is only valid when `triggerAction` is `stop` |

Spot Grid Order

| Parameter   | Type   | Required | Description                                                                                                                                                                                                                        |
| :---------- | :----- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| quoteSz | String | Conditional       | Invest amount for quote currency<br>Either `quoteSz` or `baseSz` is required             |
| baseSz | String | Conditional   | Invest amount for base currency<br>Either `quoteSz` or `baseSz` is required         |
| tradeQuoteCcy | String | No       | The quote currency for trading. Only applicable to SPOT.<br>The default value is the quote currency of instId, e.g. USD for BTC-USD.   

Contract Grid Order

| Parameter | Type   | Required | Description   |
| :-------- | :----- | :------- | :------------ |
| sz | String | Yes      | Used margin based on `USDT`            |
| direction | String | Yes   | Contract grid type<br>`long`,`short`,`neutral`    |
| lever | String | Yes   | Leverage        |
| basePos | Boolean | No   | Whether or not open a position when the strategy activates <br>Default is `false`<br>Neutral contract grid should omit the parameter     |
| tpRatio   | String | No       | Take profit ratio, 0.1 represents 10% |
| slRatio   | String | No       | Stop loss ratio, 0.1 represents 10% |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "447053782921515008",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- |:-----------------------------------------------------------|
| algoId        | String   | Algo ID                                                    |
| algoClOrdId   | String   | Client-supplied Algo ID                                    |
| sCode         | String   | The code of the event execution result, `0` means success. |
| sMsg          | String   | Rejection message if the request is unsuccessful.          |
| tag           | String   | Order tag                                                  |
