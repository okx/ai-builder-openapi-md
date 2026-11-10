---
site: okex
lang: en
name: POST / Close positions
method: POST
path: /api/v5/trade/close-position
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Close positions

Close the position of an instrument via a market order.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule (except Options): User ID + Instrument ID
#### Rate limit rule (Options only): User ID + Instrument Family

#### HTTP Request

`POST /api/v5/trade/close-position`

> Request Example

```shell
POST /api/v5/trade/close-position
body
{
    "instId":"BTC-USDT-SWAP",
    "mgnMode":"cross"
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

# Close the position of an instrument via a market order
result = tradeAPI.close_positions(
    instId="BTC-USDT-SWAP",
    mgnMode="cross"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                              |
| :-------- | :----- |:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId    | String | Yes      | Instrument ID                                                                                                                                                                                                                                            |
| posSide   | String | Conditional | Position side <br>This parameter can be omitted in `net` mode, and the default value is `net`. You can only fill with `net`.<br>This parameter must be filled in under the `long/short` mode. Fill in `long` for close-long and `short` for close-short. |
| mgnMode   | String | Yes      | Margin mode<br>`cross` `isolated`                                                                                                                                                                                                                       |
| ccy       | String | Conditional | Margin currency, required in the case of closing `cross` `MARGIN` position for `Futures mode`.                                                                                                                                                 |
| autoCxl   | Boolean| No       | Whether any pending orders for closing out needs to be automatically canceled when close position via a market order.<br>`false` or `true`, the default is `false`.                                                                                    |
| clOrdId       | String  | No       | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                               |
| tag       | String  | No       | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                         |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "clOrdId": "",
            "instId": "BTC-USDT-SWAP",
            "posSide": "long",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| instId        | String   | Instrument ID   |
| posSide       | String   | Position side   |
| clOrdId       | String  | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag       | String  | Order tag<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.           |

<aside class="notice">
if there are any pending orders for closing out and the orders do not need to be automatically canceled, it will return an error code and message to prompt users to cancel pending orders before closing the positions. <br>
</aside>
