---
site: us
lang: en
name: Get convert currencies
method: GET
path: /api/v5/asset/convert/currencies
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get convert currencies

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/convert/currencies`

> Request Example

```shell
GET /api/v5/asset/convert/currencies

```

#### Response parameters

none

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "min": "",  // Deprecated
            "max": "",  // Deprecated
            "ccy": "BTC"
        },
        {
            "min": "",
            "max": "",
            "ccy": "ETH"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type    | Description                              |
| ------ | ------- |------------------------------------------|
| ccy         | String  | Currency, e.g. BTC                       |
| min         | String  | Minimum amount to convert ( Deprecated ) |
| max         | String  | Maximum amount to convert ( Deprecated )               |
