---
site: okex
lang: en
name: Cancel withdrawal order
method: POST
path: /api/v5/fiat/cancel-withdrawal
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---


### Cancel withdrawal order

Cancel a pending fiat withdrawal order, currently only applicable to TRY

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/fiat/cancel-withdrawal`

> Request Example

```shell
 POST /api/v5/fiat/cancel-withdrawal
 body
 {
    "ordId": "124041201450544699"
 }
```

```python

```

#### Request Parameters

| **Parameters**    | **Types** | **Required** | **Description**    |
|:------------------| :-------- | :----------- |:-------------------|
| ordId             | String    | Yes          | Payment Order Id   |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "ordId": "124041201450544699",
        "state": "canceled"
    }
  ]
}

```

#### Response Parameters

| Parameter    | Type    | Description                                  |
|--------------|---------|----------------------------------------------|
| ordId        | String  | Payment Order ID                             |
| state        | String  | The state of the transaction, e.g.`canceled` |
