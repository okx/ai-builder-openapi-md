---
site: okex
lang: en
name: POST / Place sub order
method: POST
path: /api/v5/tradingBot/signal/sub-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Place sub order

You can place an order only if you have sufficient funds.<br><br>

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/sub-order`

> Request Example

```shell
POST /api/v5/tradingBot/signal/sub-order
body
{
    "algoId":"1222",
    "instId":"BTC-USDT-SWAP",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                                                                                                |
| :--------- | :------ | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId            | String   | Yes       | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                        |
| algoId            | String   | Yes       | Algo ID                                                                                                                                                                                    |
| side              | String   | Yes       | Order side, `buy` `sell`                                                                                                                                                                   |
| ordType           | String   | Yes       | Order type <br>`market`: Market order <br>`limit`: Limit order                                                                                                                             |
| sz                | String   | Yes       | Quantity to buy or sell                                                                                                                                                                    |
| px                | String   | Conditional     | Order price. Only applicable to `limit` order.                                                                                                                                             |
| reduceOnly        | Boolean  | No       | Whether orders can only reduce in position size. <br>Valid options: `true` or `false`. The default value is `false`. <br>Only applicable to `Futures mode`/`Multi-currency margin` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| code       | String   | The result code, `0` means success |
| msg        | String   | The error message, empty if the code is 0  |
| data       | Array of objects   | Array of objects contains the response results              |

<aside class="notice">
ordType<br>
Order type. When creating a new order, you must specify the order type. The order type you specify will affect: 1) what order parameters are required, and 2) how the matching system executes your order. The following are valid order types:<br>
`limit`: Limit order, which requires specified sz and px.<br>
`market`: Market order. It will be filled with market price (by swiping opposite order book). Market order will be placed to order book with most aggressive price allowed by Price Limit Mechanism.
</aside>

<aside class="notice">
sz refers to the number of contracts。
</aside>

<aside class="notice">
reduceOnly<br>
When placing an order with this parameter set to true, it means that the order will reduce the size of the position only
The sum of the current order size and all reverse direction reduce-only pending orders which's price-time priority is higher than the current order, cannot exceed the contract quantity of position.
Only applicable to `Futures mode` and `Multi-currency margin`
</aside>
