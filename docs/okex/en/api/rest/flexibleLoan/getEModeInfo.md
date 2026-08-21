---
site: okex
lang: en
name: GET / E-Mode info
method: GET
path: /api/v5/finance/flexible-loan/emode-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / E-Mode info

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP
#### Permission: Read

#### HTTP Request

`GET /api/v5/finance/flexible-loan/emode-info`

> Request Example

```shell
GET /api/v5/finance/flexible-loan/emode-info
```

#### Request Parameters

None.

Returns all currently valid collateral and borrow currency pairs eligible for E-Mode. This endpoint does not require authentication.

> Response Example

```json
{
  "code": "0",
  "data": [
    {"collateralCcy": "USDC", "borrowCcy": "USDT"},
    {"collateralCcy": "WBTC", "borrowCcy": "BTC"}
  ],
  "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
|:--|:--|:--|
| collateralCcy | String | Collateral currency eligible for E-Mode. |
| borrowCcy | String | Borrow currency eligible with `collateralCcy`. |
