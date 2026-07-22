---
site: us
lang: en
name: Get withdrawal order detail
method: GET
path: /api/v5/fiat/withdrawal
module: fiat
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---


### Get withdrawal order detail

Get fiat withdraw order detail

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/withdrawal`

> Request Example

```shell
 GET /api/v5/fiat/withdrawal?ordId=024041201450544699
 body
 {
    "ordId": "024041201450544699"
 }
```

```python

```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description** |
|:---------------| :-------- | :----------- |:----------------|
| ordId          | String    | Yes          | Order ID        |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "100",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    }
  ]
}

```

#### Response Parameters

| Parameter       | Type    | Description                                                                                                    |
|-----------------|---------|----------------------------------------------------------------------------------------------------------------|
| ordId           | String  | Order ID                                                                                            |
| clientId        | String  | The original request ID associated with the transaction                                                        |
| ccy             | String  | The currency of the transaction                                                                                |
| amt             | String  | Amount of the transaction                                                                                      |
| fee             | String  | The transaction fee                                                                                            |
| paymentAcctId   | String  | The ID of the payment account used                                                                             |
| paymentMethod   | String  | Payment method, e.g. `TR_BANKS`<br>`PIX`<br>`SEPA`<br>`XPULSE`<br>`NPP`<br>`US_WIRE`<br>`SG_FAST`              |
| state           | String  | The state of the transaction<br>`completed`<br>`failed`<br>`pending`<br>`canceled`<br>`inqueue`<br>`processing` |
| cTime           | String  | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`              |
| uTime           | String  | The update time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`                |
