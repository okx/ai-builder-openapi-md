---
site: okex
lang: en
name: POST / Cancel sub order
method: POST
path: /api/v5/tradingBot/signal/cancel-sub-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Cancel sub order

Cancel an incomplete order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/cancel-sub-order`

> Request Example

```shell
POST /api/v5/tradingBot/signal/cancel-sub-order
body
{
    "algoId":"91664",
    "signalOrdId":"590908157585625111",
    "instId":"BTC-USDT-SWAP"
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId  | String | Yes    | Algo ID                      |
| instId  | String | Yes    | Instrument ID, e.g. BTC-USDT-SWAP        |
| signalOrdId | String | Yes     | Order ID                    |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "signalOrdId":"590908157585625111",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| code       | String   | The result code, `0` means success |
| msg        | String   | The error message, empty if the code is 0   |
| data       | Array of objects   | Array of objects contains the response results              |
| > signalOrdId      | String   | Order ID                    |
| > sCode            | String   | The code of the event execution result, `0` means success. |
| > sMsg             | String   | Rejection or success message of event execution.          |

<aside class="notice">
Cancel order returns with sCode equal to 0. It is not strictly considered that the order has been canceled. It only means that your cancellation request has been accepted by the system server. The result of the cancellation is subject to the state by get sub orders endpoint.<br>
</aside>
