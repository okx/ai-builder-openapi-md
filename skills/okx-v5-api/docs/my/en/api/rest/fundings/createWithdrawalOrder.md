---
site: my
lang: en
name: Create withdrawal order
method: POST
path: /api/v5/fiat/create-withdrawal
module: fiat
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---


### Create withdrawal order

Initiate a fiat withdrawal request (Authenticated endpoint, Only for API keys with "Withdrawal" access)<br>
Only supported withdrawal of assets from funding account.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/fiat/create-withdrawal`

> Request Example

```shell
 POST /api/v5/fiat/create-withdrawal
 body
 {
    "paymentAcctId": "412323",
    "ccy": "TRY",
    "amt": "10000",
    "paymentMethod": "TR_BANKS",
    "clientId": "194a6975e98246538faeb0fab0d502df"
 }
```

```python

```

#### Request Parameters

| **Parameters** | **Type** | **Required** | **Description**                                                                                                                                                    |
| :------------- | :-------- |:-------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| paymentAcctId  | String    | Yes          | Payment account id to withdraw to, retrieved from get withdrawal payment methods API                                                                               |
| ccy            | String    | Yes          | Currency for withdrawal, must match currency allowed for paymentMethod                                                                                             |
| amt            | String    | Yes          | Requested withdrawal amount before fees. Has to be less than or equal to 2 decimal points double                                                                   |
| paymentMethod  | String    | Yes          | Payment method to use for withdrawal<br>`TR_BANKS`<br>`PIX`<br>`SEPA`<br>`XPULSE`<br>`NPP`<br>`US_WIRE`<br>`SG_FAST`                                                |
| clientId       | String    | Yes          | Client-supplied ID, A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters <br> e.g. `194a6975e98246538faeb0fab0d502df` |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "cTime": "1707429385000",
        "uTime": "1707429385000",
        "ordId": "124041201450544699",
        "paymentMethod": "TR_BANKS",
        "paymentAcctId": "20",
        "fee": "0",
        "amt": "100",
        "ccy": "TRY",
        "state": "completed",
        "clientId": "194a6975e98246538faeb0fab0d502df"
    }
  ]
}

```

#### Response Parameters

| Parameter              | Type    | Description                                                                                       |
|------------------------|---------|---------------------------------------------------------------------------------------------------|
| ordId             | String  | The unique order Id                                                                               |
| clientId          | String  | The client ID associated with the transaction                                                     |
| amt               | String  | The requested amount for the transaction                                                          |
| ccy               | String  | The currency of the transaction                                                                   |
| fee               | String  | The transaction fee                                                                               |
| paymentAcctId     | String  | The Id of the payment account used                                                                |
| paymentMethod     | String  | Payment Method<br>`TR_BANKS`<br>`PIX`<br>`SEPA`                                                   |
| state             | String  | The State of the transaction<br>`processing`<br>`completed`                                                                  |
| cTime             | String  | The creation time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime             | String  | The update time of the transaction, Unix timestamp format in milliseconds, e.g. `1597026383085`   |
