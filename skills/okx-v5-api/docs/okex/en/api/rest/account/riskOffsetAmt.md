---
site: okex
lang: en
name: Set risk offset amount
method: POST
path: /api/v5/account/set-riskOffset-amt
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set risk offset amount
Set risk offset amount. This does not represent the actual spot risk offset amount. Only applicable to Portfolio Margin Mode.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/account/set-riskOffset-amt`

> Request Example

```shell
# Set spot risk offset amount
POST /api/v5/account/set-riskOffset-amt
body
{
   "ccy": "BTC",
   "clSpotInUseAmt": "0.5"
}

```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| ccy | String | Yes | Currency |
| clSpotInUseAmt | String | Yes | Spot risk offset amount defined by users |

> Response Example

```json
{
   "code": "0",
   "msg": "",
   "data": [
      {
         "ccy": "BTC",
         "clSpotInUseAmt": "0.5"
      }
   ]
}
```

#### Response Parameters

| Parameters | Types | Description |
| --- | --- | --- |
| ccy | String | Currency |
| clSpotInUseAmt | String | Spot risk offset amount defined by users |
