---
site: okex
lang: en
name: POST / Cancel algo order
method: POST
path: /api/v5/trade/cancel-algos
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Cancel algo order

Cancel unfilled algo orders. A maximum of 10 orders can be canceled per request. Request parameters should be passed in the form of an array.

#### Rate Limit: 20 orders per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

#### HTTP Request

`POST /api/v5/trade/cancel-algos`

> Request Example

```shell
POST /api/v5/trade/cancel-algos
body
[
    {
        "algoId":"590919993110396111",
        "instId":"BTC-USDT"
    },
    {
        "algoId":"590920138287841222",
        "instId":"BTC-USDT"
    }
]
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Cancel unfilled algo orders (not including Iceberg order, TWAP order, Trailing Stop order)
algo_orders = [
    {"instId": "BTC-USDT", "algoId": "590919993110396111"},
    {"instId": "BTC-USDT", "algoId": "590920138287841222"}
]

result = tradeAPI.cancel_algo_order(algo_orders)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                     |
| :----- | :----- | :------- | :----------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USDT` |
| algoId | String | Conditional       | Algo ID<br>Either `algoId` or `algoClOrdId` is required. If both are passed, `algoId` will be used. |
| algoClOrdId | String | Conditional       | Client-supplied Algo ID<br>Either `algoId` or `algoClOrdId` is required. If both are passed, `algoId` will be used. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1836489397437468672",
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

| **Parameter** | **Type** | **Description**                                            |
|:--------------|:---------|:-----------------------------------------------------------|
| algoId        | String   | Algo ID                                                    |
| sCode         | String   | The code of the event execution result, `0` means success. |
| sMsg          | String   | Rejection message if the request is unsuccessful.          |
| clOrdId       | String   | <del>Client Order ID as assigned by the client</del>(Deprecated)     |
| algoClOrdId     | String   | <del>Client-supplied Algo ID</del>(Deprecated)                                    |
| tag           | String   | <del>Order tag  </del>(Deprecated)                                                                                                                                                                                                                                                                  |
