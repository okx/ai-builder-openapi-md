---
site: okex
lang: en
name: POST / Copy grid algo order
method: POST
path: /api/v5/tradingBot/grid/copy-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Copy grid algo order
#### Rate Limit: 20 requests per 2 seconds
#### Rate Limit Rule: User ID + Instrument ID
#### HTTP Request

`POST /api/v5/tradingBot/grid/copy-order-algo`

> Request Example

```shell
# Spot grid copy
POST /api/v5/tradingBot/grid/copy-order-algo
body
{
    "instId": "BTC-USDT",
    "algoOrdType": "grid",
    "sourceAlgoId": "580007082221121536",
    "quoteSz": "1000"
}
```
```shell
# Contract grid copy
POST /api/v5/tradingBot/grid/copy-order-algo
body
{
    "instId": "BTC-USDT-SWAP",
    "algoOrdType": "contract_grid",
    "sourceAlgoId": "580007082221121536",
    "lever": "3",
    "autoReserve": true,
    "sz": "5000"
}
```

#### Request Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| instId | String | Yes | Instrument ID, e.g. `BTC-USDT` |
| algoOrdType | String | Yes | Algo order type<br>`grid`: Spot grid<br>`contract_grid`: Contract grid |
| sourceAlgoId | String | Yes | Lead algo order ID to follow and copy from |
| quoteSz | String | No | Quote currency investment amount<br>Only applicable to `grid` |
| lever | String | No | Leverage<br>Only applicable to `contract_grid` |
| autoReserve | Boolean | No | Whether to auto-reserve margin. Only applicable to `contract_grid`<br>`true`: Actual margin and extra margin are automatically calculated based on `sz`<br>`false`: Manually specify `actualMarginSz` and `extraMarginSz` |
| sz | String | No | Total investment amount in USDT. Required when `autoReserve` is `true`<br>Only applicable to `contract_grid` |
| actualMarginSz | String | No | Actual margin. Required when `autoReserve` is `false`<br>Only applicable to `contract_grid` |
| extraMarginSz | String | No | Extra margin reserved. Defaults to `0` when unspecified<br>Only applicable to `contract_grid` |
| algoClOrdId | String | No | Client-supplied Algo ID  |
| tag | String | No | Order tag |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "581234567890123456",
            "algoClOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ]
}
```

#### Response Parameters
| Parameter | Type | Description |
| :--- | :--- | :--- |
| algoId | String | Algo order ID |
| algoClOrdId | String | Client-supplied Algo ID |
| sCode | String | Event execution status code, `0` indicates success |
| sMsg | String | Error message if the event execution failed |
| tag | String | Order tag |
