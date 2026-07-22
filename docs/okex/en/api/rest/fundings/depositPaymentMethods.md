---
site: okex
lang: en
name: Get deposit payment methods
method: GET
path: /api/v5/fiat/deposit-payment-methods
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get deposit payment methods

To display all the available fiat deposit payment methods

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/deposit-payment-methods`

> Request Example

```shell
GET /api/v5/fiat/deposit-payment-methods?ccy=TRY
body
{
  "ccy" : "TRY",
}
```

```python

```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                           |
| :------------- | :-------- | :----------- |:----------------------------------------------------------|
| ccy            | String    | Yes          | Fiat currency, ISO-4217 3 digit currency code, e.g. `TRY` |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ccy": "TRY",
      "paymentMethod": "TR_BANKS",
      "feeRate": "0",
      "minFee": "0",
      "limits": {
        "dailyLimit": "2147483647",
        "dailyLimitRemaining": "2147483647",
        "weeklyLimit": "2147483647",
        "weeklyLimitRemaining": "2147483647",
        "monthlyLimit": "",
        "monthlyLimitRemaining": "",
        "maxAmt": "1000000",
        "minAmt": "1",
        "lifetimeLimit": "2147483647"
      },
      "accounts": [
          {
            "paymentAcctId": "1",
            "acctNum": "TR740001592093703829602611",
            "recipientName": "John Doe",
            "bankName": "VakıfBank",
            "bankCode": "TVBATR2AXXX",
            "state": "active"
          },
          {
            "paymentAcctId": "2",
            "acctNum": "TR740001592093703829602622",
            "recipientName": "John Doe",
            "bankName": "FBHLTRISXXX",
            "bankCode": "",
            "state": "active"
          }
      ]
    }
  ]
}

```

#### Response Parameters

| Parameter               | Type            | Description                                                                                                           |
|-------------------------|-----------------|-----------------------------------------------------------------------------------------------------------------------|
| ccy                     | String          | Fiat currency                                                                                                         |
| paymentMethod           | String          | The payment method associated with the currency<br>`TR_BANKS`<br>`PIX`<br>`SEPA`<br>`XPULSE`<br>`NPP`<br>`US_WIRE`    |
| feeRate                 | String          | The fee rate for each deposit, expressed as a percentage<br>e.g. `0.02` represents 2 percent fee for each transaction. |
| minFee                  | String          | The minimum fee for each deposit                                                                                      |
| limits                  | Object          | An object containing limits for various transaction intervals                                                         |
| > dailyLimit            | String          | The daily transaction limit                                                                                           |
| > dailyLimitRemaining   | String          | The remaining daily transaction limit                                                                                 |
| > weeklyLimit           | String          | The weekly transaction limit                                                                                          |
| > weeklyLimitRemaining  | String          | The remaining weekly transaction limit                                                                                |
| > monthlyLimit          | String          | The monthly transaction limit                                                                                         |
| > monthlyLimitRemaining | String          | The remaining monthly transaction limit                                                                               |
| > maxAmt                | String          | The maximum amount allowed per transaction                                                                            |
| > minAmt                | String          | The minimum amount allowed per transaction                                                                            |
| > lifetimeLimit         | String          | The lifetime transaction limit. Return the configured value, "" if not configured                                     |
| accounts                | Array of Object | An array containing information about payment accounts associated with the currency and method.                       |
| > paymentAcctId         | String          | The account ID for withdrawal                                                                                         |
| > acctNum               | String          | The account number, which can be an IBAN or other bank account number.                                                |
| > recipientName         | String          | The name of the recipient                                                                                             |
| > bankName              | String          | The name of the bank associated with the account                                                                      |
| > bankCode              | String          | The SWIFT code / BIC / bank code associated with the account                                                          |
| > state                 | String          | The state of the account<br>`active`                                                                                  |
