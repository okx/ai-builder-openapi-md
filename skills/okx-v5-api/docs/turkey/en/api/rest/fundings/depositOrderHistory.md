---
site: turkey
lang: en
name: Get deposit order history
method: GET
path: /api/v5/fiat/deposit-order-history
module: fiat
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---


### Get deposit order history

Get fiat deposit order history

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/deposit-order-history`

> Request Example

```shell
 GET /api/v5/fiat/deposit-order-history

```

```python

```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                                              |
| :------------- | :-------- | :----------- |:-----------------------------------------------------------------------------------------------------------------------------|
| ccy            | String    | No          | ISO-4217 3 digit currency code                                                                                               |
| paymentMethod  | String    | No          | Payment Method<br>`TR_BANKS`<br>`PIX`<br>`SEPA`<br>`XPULSE`<br>`NPP`<br>`US_WIRE`                                            |
| state          | String    | No          | State of the order<br>`completed`<br>`failed`<br>`pending`<br>`canceled`<br>`inqueue`<br>`processing`                        |
| after          | String    | No          | Filter with a begin timestamp. Unix timestamp format in milliseconds (inclusive), e.g. `1597026383085`                       |
| before         | String    | No          | Filter with an end timestamp. Unix timestamp format in milliseconds (inclusive), e.g. `1597026383085`                        |
| limit          | String    | No          | Number of results per request. Maximum and default is 100                                                                    |

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
        "amt": "10000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    },
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "024041201450544690",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "amt": "50000",
        "fee": "0",
        "ccy": "TRY",
        "state": "completed",
        "clientId": ""
    }
  ]
}

```

#### Response Parameters

| Parameter              | Type    | Description                                                                                   |
|------------------------|---------|-----------------------------------------------------------------------------------------------|
| ordId                  | String  | Unique Order ID                                                                               |
| clientId               | String  | Client Id of the transaction                                                                  |
| ccy                    | String  | Currency of the transaction                                                                   |
| amt                    | String  | Final amount of the transaction                                                               |
| fee                    | String  | Transaction fee                                                                               |
| paymentAcctId          | String  | ID of the payment account used                                                                |
| paymentMethod          | String  | Payment Method, e.g. `TR_BANKS`                                                               |
| state                  | String  | State of the transaction<br>`completed`<br>`failed`<br>`pending`<br>`canceled`<br>`inqueue`   |
| cTime                  | String  | Creation time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime                  | String  | Update time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
