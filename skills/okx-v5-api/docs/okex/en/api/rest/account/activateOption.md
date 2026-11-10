---
site: okex
lang: en
name: Activate option
method: POST
path: /api/v5/account/activate-option
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Activate option

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/activate-option`

> Request Example

```shell
POST /api/v5/account/activate-option

```

#### Request Parameters
None

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "ts": "1600000000000"
    }]
}
```

#### Response Parameters

| Parameter | Type   | Description     |
| :-------- | :----- |:----------------|
| ts   | String | Activation time |
