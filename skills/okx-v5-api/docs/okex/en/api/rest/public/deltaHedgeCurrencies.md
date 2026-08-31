---
site: okex
lang: en
name: Get Delta hedge currencies
method: GET
path: /api/v5/public/delta-hedge-currencies
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get Delta hedge currencies

Retrieve the currencies that share the same underlying asset and can therefore form a Delta hedge relationship, e.g. `ETH` and `BETH`, or `AAPL` and `XAAPL`.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/delta-hedge-currencies`

> Request Example

```shell
GET /api/v5/public/delta-hedge-currencies

GET /api/v5/public/delta-hedge-currencies?ccy=ETH
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve Delta hedge currencies
result = publicDataAPI.get_delta_hedge_currencies(
    ccy="ETH"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description |
|:----------|:-------|:---------|:------------|
| ccy       | String | No       | Currency, e.g. `ETH`.<br>When specified, only the mapping entry for that currency is returned.<br>When not specified, the full mapping is returned. |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ccy": "ETH",
            "hedgeCcy": ["BETH"]
        },
        {
            "ccy": "XAU",
            "hedgeCcy": ["XAUT"]
        },
        {
            "ccy": "AAPL",
            "hedgeCcy": ["XAAPL"]
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
|:----------|:-------|:------------|
| ccy       | String | Currency, e.g. `AAPL`, `ETH` |
| hedgeCcy  | Array of strings | Currencies that share the same underlying asset as `ccy` and can form a Delta hedge relationship with it, e.g. `["XAAPL"]` when `ccy` is `AAPL`. The relationship is symmetric. |
