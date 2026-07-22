---
site: okex
lang: en
name: POST / Amend algo order
method: POST
path: /api/v5/trade/amend-algos
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Amend algo order

Amend unfilled algo orders (Support Stop order and Trigger order only, not including Move_order_stop order, Iceberg order, TWAP order, Trailing Stop order).<br>

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/trade/amend-algos`

> Request Example

```shell
POST /api/v5/trade/amend-algos
body
{
    "algoId":"2510789768709120",
    "newSz":"2",
    "instId":"BTC-USDT"
}
```

#### Request Parameters

| Parameter | Type   | Required        | Description                                                                                                                                                                                                                                                                                       |
| :----- | :----- |:----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                | String    | Yes             | Instrument ID                                                                                                                                                                                                                                                                                     |
| algoId                | String    | Conditional        | Algo ID<br>Either `algoId` or `algoClOrdId` is required. If both are passed, `algoId` will be used.                                                                                                                                                                                               |
| algoClOrdId           | String    | Conditional        | Client-supplied Algo ID<br>Either `algoId` or `algoClOrdId` is required. If both are passed, `algoId` will be used.                                                                                                                                                                               |
| cxlOnFail             | Boolean   | No              | Whether the order needs to be automatically canceled when the order amendment fails  <br>Valid options: `false` or `true`, the default is `false`.                                                                                                                                                |
| reqId                 | String    | Conditional     | Client Request ID as assigned by the client for order amendment <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. <br>The response will include the corresponding `reqId` to help you identify the request if you provide it in the request. |
| newSz                 | String    | Conditional     | New quantity after amendment and it has to be larger than 0.                                                                                                                                                                                                                                      |

<b>Take Profit / Stop Loss Order</b>

| Parameter | Type   | Required        | Description                                                                                                                                                                                                                                                                                       |
| :----- | :----- |:----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newTpTriggerPx        | String    | Conditional     | Take-profit trigger price. <br>Either the take-profit trigger price or order price is 0, it means that the take-profit is deleted                                                                                                                                                                 |
| newTpOrdPx            | String    | Conditional     | Take-profit order price <br>If the price is -1, take-profit will be executed at the market price.                                                                                                                                                                                                 |
| newSlTriggerPx        | String    | Conditional     | Stop-loss trigger price.<br>Either the stop-loss trigger price or order price is 0, it means that the stop-loss is deleted                                                                                                                                                                        |
| newSlOrdPx            | String    | Conditional     | Stop-loss order price <br>If the price is -1, stop-loss will be executed at the market price.                                                                                                                                                                                                     |
| newTpTriggerPxType    | String    | Conditional     | Take-profit trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price                                                                                                                                                                                              |
| newSlTriggerPxType    | String    | Conditional     | Stop-loss trigger price type<br>`last`: last price <br>`index`: index price <br>`mark`: mark price                                                                                                                                                                                                |

<b>Trigger Order</b>

| Parameter | Type   | Required        | Description                                                                                                                                                                                                                                                                                       |
| :----- | :----- |:----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newTriggerPx           | String | Yes       | New trigger price after amendment                                                                                                                                                                                                                                                             |
| newOrdPx             | String | Yes       | New order price after amendment<br>If the price is `-1`, the order will be executed at the market price.                                                                                                                                                                                      |
| newTriggerPxType       | String | No   | New trigger price type after amendment <br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                     |
| attachAlgoOrds      | Array of objects | No     | Attached TP/SL or trailing stop order info<br>Applicable to `Futures mode/Multi-currency margin/Portfolio margin`                                                                                                                                                                 |
| > newTpTriggerPx       | String | No       | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                        |
| > newTpTriggerRatio    | String | No       | Take profit trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP. <br> Only one of `newTpTriggerPx` and `newTpTriggerRatio` can be passed <br> If the main order is a buy order, it must be greater than 0, and if the main order is a sell order, it must be between -1 and 0. <br> 0 means to delete the take-profit.                                                                                                                                                                                                            |
| > newTpTriggerPxType   | String | No       | Take-profit trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price<br>The default is `last`                                                                                                                                             |
| > newTpOrdPx           | String | No       | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                            |
| > newSlTriggerPx       | String | No       | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                     |
| > newSlTriggerRatio    | String | No       | Stop-loss trigger ratio, 0.3 represents 30% <br> Only applicable to FUTURES and SWAP. <br> Only one of `newSlTriggerPx` and `newSlTriggerRatio` can be passed <br> If the main order is a buy order, it must be between 0 and 1, and if the main order is a sell order, it must be greater than 0. <br> 0 means to delete the stop-loss.                                                                                                                                                                                                        |
| > newSlTriggerPxType   | String | No       | Stop-loss trigger price type<br>`last`: last price<br>`index`: index price<br>`mark`: mark price <br>The default is `last`                                                                                                                                                 |
| > newSlOrdPx           | String | No       | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                         |
| > newCallbackRatio    | String | Conditional | New callback ratio, e.g. `0.05` represents 5%.<br>Either `newCallbackRatio` or `newCallbackSpread` can be passed. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > newCallbackSpread   | String | Conditional | New callback spread (price distance).<br>Either `newCallbackRatio` or `newCallbackSpread` can be passed. Only one can be passed.<br>Only applicable when `ordType` = `move_order_stop` |
| > newActivePx         | String | No          | New activation price.<br>Only applicable when `ordType` = `move_order_stop` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId":"algo_01",
            "algoId":"2510789768709120",
            "reqId":"po103ux",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                      |
| :--------- | :------- | :---------------------------- |
| algoId         | String   | Algo ID                                                           |
| algoClOrdId    | String   | Client-supplied Algo ID                                           |
| reqId      | String   | Client Request ID as assigned by the client for order amendment.      |
| sCode      | String   | The code of the event execution result, `0` means success. |
| sMsg       | String   | Rejection message if the request is unsuccessful.          |
