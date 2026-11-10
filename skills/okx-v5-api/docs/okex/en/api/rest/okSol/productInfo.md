---
site: okex
lang: en
name: GET / Product info
method: GET
path: /api/v5/finance/staking-defi/sol/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Product info

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/sol/product-info`

> Request Example

```shell
GET /api/v5/finance/staking-defi/sol/product-info

```

```python

```

> Response Example

```json
{
    "code": "0",
    "data": {
        "fastRedemptionAvail": "240",
        "fastRedemptionDailyLimit": "240",
        "rate": "5.57",
        "redemptDays": "2",
        "minAmt": "0.01"
    },
    "msg": ""
}
```

#### Response Parameters

| Parameter                    | Type   | Description                                                                             |
|------------------------------|--------|-----------------------------------------------------------------------------------------|
| fastRedemptionDailyLimit     | String | Fast redemption daily limit<br>The master account and sub-accounts share the same limit |
| fastRedemptionAvail          | String | Currently fast redemption max available amount                                          |
| rate                         | String | Latest OKSOL APY                                                                        |
| redemptDays                  | String | Redemption days of OKSOL                                                                |
| minAmt                       | String | Minimum subscription amount of OKSOL                                                    |
