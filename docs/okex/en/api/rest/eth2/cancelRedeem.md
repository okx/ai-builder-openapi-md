---
site: okex
lang: en
name: POST / Cancel redeem
method: POST
path: /api/v5/finance/staking-defi/eth/cancel-redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Cancel redeem

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/staking-defi/eth/cancel-redeem`

> Request Example

```shell
POST /api/v5/finance/staking-defi/eth/cancel-redeem
body
{
    "ordId": "1234567890"
}
```

#### Request Parameters

| Parameter      | Type   | Required | Description                                                                                                            |
|:---------------|:-------|:---------|:-----------------------------------------------------------------------------------------------------------------------|
| ordId           | String | Yes       | Order ID                                                                                         |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234567890"
        }
    ],
    "msg": ""
}
```

#### Response Parameters
| Parameter | Type   | Description |
|-----------|--------|-------------|
| ordId     | String | Order ID    |
