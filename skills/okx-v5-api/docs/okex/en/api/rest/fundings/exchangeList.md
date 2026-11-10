---
site: okex
lang: en
name: Get exchange list (public)
method: GET
path: /api/v5/asset/exchange-list
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get exchange list (public)

Authentication is not required for this public endpoint.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/asset/exchange-list`

> Request Example

```shell
GET /api/v5/asset/exchange-list

```

```python

```

#### Request Parameters

None

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "exchId": "did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1",
        "exchName": "1xbet"
    }
  ]
}
```

#### Response Parameters

| Parameter    | Type   | Description                                 |
| :-------- | :----- |:--------------------------------------------|
| exchName       | String | Exchange name, e.g. `1xbet`                 |
| exchId       | String | Exchange ID, e.g. `did:ethr:0xfeb4f99829a9acdf52979abee87e83addf22a7e1`                          |
