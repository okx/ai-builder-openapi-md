---
site: okex
lang: en
name: Get MM instrument types
method: GET
path: /api/v5/public/mm-instrument-types
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get MM instrument types

Retrieve the list of MM Program instrument type classifications for SPOT and SWAP instruments.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/mm-instrument-types`

> Request Example

```shell
GET /api/v5/public/mm-instrument-types?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve the list of MM Program instrument type classifications
result = publicDataAPI.get_mm_instrument_types(
    instType="SWAP"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description |
|:----------|:-------|:---------|:------------|
| instType  | String | No       | Instrument type.<br>`SPOT`<br>`SWAP`<br>When not specified, returns all types. |
| instId    | String | No       | Instrument ID, e.g. `BTC-USDT`, `BTC-USDT-SWAP`.<br>When specified, returns at most one record. |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "A"
        },
        {
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "A"
        },
        {
            "instId": "XAU-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "B-TradFi"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
|:----------|:-------|:------------|
| instId    | String | Instrument ID, e.g. `BTC-USDT-SWAP` |
| instType  | String | Instrument type.<br>`SPOT`<br>`SWAP` |
| pairType  | String | MM Program classification type.<br>`A`: High liquidity tier<br>`B-Crypto`: Medium/low liquidity crypto assets<br>`B-TradFi`: Traditional finance instruments (SWAP only) |
