---
site: turkey
lang: en
name: POST / Place algo order
method: POST
path: /api/v5/trade/order-algo
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Place algo order

The algo order includes `trigger` order, `oco` order, `conditional` order and trailing order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/trade/order-algo`

> Request Example

```shell
# Place Take Profit / Stop Loss Order
POST /api/v5/trade/order-algo
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}

# Place Trigger Order
POST /api/v5/trade/order-algo
body
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "trigger",
    "sz": "10",
    "triggerPx": "100",
    "orderPx": "-1"
}

# Place Trailing Stop Order
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT",
	"tdMode": "cash",
	"side": "buy",
	"ordType": "move_order_stop",
	"sz": "10",
	"callbackRatio": "0.05"
}
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# One-way stop order
result = tradeAPI.place_algo_order(
    instId="BTC-USDT",
    tdMode="cash",
    side="buy",
    ordType="conditional",
    sz="2",
    tpTriggerPx="15",
    tpOrdPx="18"
)
print(result)
```

#### Request Parameters

| Parameter     | Type    | Required  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:--------------|:--------|:----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String  | Yes       | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| tdMode        | String  | Yes       | Trade mode<br>`cash`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| side          | String  | Yes       | Order side<br>`buy`<br>`sell`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ordType       | String  | Yes       | Order type<br>`conditional`: One-way stop order<br>`oco`: One-cancels-the-other order<br>`trigger`: Trigger order<br>`move_order_stop`: Trailing order                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| sz            | String  | Yes       | Quantity to buy or sell                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| tag           | String  | No        | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| tgtCcy        | String  | No        | Order quantity unit setting for `sz`<br>`base_ccy`: Base currency ,`quote_ccy`: Quote currency<br>Only applicable to `SPOT` traded with Market buy `conditional` order<br>Default is `quote_ccy` for buy, `base_ccy` for sell                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| algoClOrdId   | String | No        | Client-supplied Algo ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

<b>Take Profit / Stop Loss Order</b>

<aside class="notice">
Predefine the price you want the order to trigger a market order to execute immediately or it will place a limit order. <br>This type of order will not freeze your free margin in advance.
</aside>

learn more about [Take Profit / Stop Loss Order](/help/11015447687437)

| Parameter       | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|:----------------|:-------|:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tpTriggerPx     | String | No       | Take-profit trigger price<br>If you fill in this parameter, you should fill in the take-profit order price as well.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| tpTriggerPxType | String | No       | Take-profit trigger price type<br>`last`: last price<br>The default is `last`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| tpOrdPx         | String | No       | Take-profit order price<br>If you fill in this parameter, you should fill in the take-profit trigger price as well. <br>If the price is `-1`, take-profit will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| slTriggerPx     | String | No       | Stop-loss trigger price<br>If you fill in this parameter, you should fill in the stop-loss order price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| slTriggerPxType | String | No       | Stop-loss trigger price type<br>`last`: last price<br>The default is `last`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| slOrdPx         | String | No       | Stop-loss order price <br>If you fill in this parameter, you should fill in the stop-loss trigger price. <br>If the price is `-1`, stop-loss will be executed at the market price.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

<aside class="notice">
Take Profit / Stop Loss Order<br>
When placing net TP/SL order (ordType=conditional) and both take-profit and stop-loss parameters are sent, only stop-loss logic will be performed and take-profit logic will be ignored.
</aside>

<b>Trigger Order</b>

<aside class="notice">
Use a trigger order to place a market or limit order when a specific price level is crossed. <br>When a Trigger Order is triggered, if your account balance is lower than the order amount, the system will automatically place the order based on your current balance. <br>Trigger orders do not freeze assets when placed. 
</aside>

learn more about [Trigger Order](/help/11015447687437)

| Parameter           | Type   | Required | Description                                                                                                                                                                                                                                                            |
|:--------------------| :----- |:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| triggerPx           | String | Yes      | Trigger price                                                                                                                                                                                                                                                          |
| orderPx             | String | Yes      | Order Price <br>If the price is `-1`, the order will be executed at the market price.                                                                                                                                                                                  |
| triggerPxType       | String | No       | Trigger price type<br>`last`: last price<br>The default is `last`                                                                                                                                                       |

<b>Trailing Stop Order</b>

<aside class="notice">
A trailing stop order is a stop order that tracks the market price. Its trigger price changes with the market price. Once the trigger price is reached, a market order is placed.<br>
Actual trigger price for sell orders and short positions = Highest price after order placement – Trail variance (Var.), or Highest price after placement × (1 – Trail variance) (Ratio).<br>
Actual trigger price for buy orders and long positions = Lowest price after order placement + Trail variance, or Lowest price after order placement × (1 + Trail variance).<br>
You can use the activation price to set the activation condition for a trailing stop order.
</aside>

learn more about [Trailing Stop Order](/help/11015447687437)

| Parameter         | Type   | Required    | Description                                                                                                                                                                                                                                                                                                                                                                                                                          |
|:------------------| :----- |:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| callbackRatio     | String | Conditional | Callback price ratio, e.g. `0.01` represents `1%`<br>Either `callbackRatio` or `callbackSpread` is allowed to be passed.                                                                                                                                                                                                                                                                                                             |
| callbackSpread    | String | Conditional | Callback price variance                                                                                                                                                                                                                                                                                                                                                                                                              |
| activePx          | String | No          | Active price<br>The system will only start tracking the market and calculating your trigger price after the activation price is reached. If you don’t set a price, your order will be activated as soon as it’s placed.                                                                                                                                                                                                              |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "order1234",
            "algoId": "1836487817828872192",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                                            |
|:----------------|:---------|:-----------------------------------------------------------|
| algoId          | String   | Algo ID                                                    |
| clOrdId         | String   | <del>Client Order ID as assigned by the client</del>(Deprecated)     |
| algoClOrdId     | String   | Client-supplied Algo ID                                    |
| sCode           | String   | The code of the event execution result, `0` means success. |
| sMsg            | String   | Rejection message if the request is unsuccessful.          |
