---
site: okex
lang: en
name: GET / Total profit sharing
method: GET
path: /api/v5/copytrading/total-profit-sharing
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Total profit sharing

The leading trader gets the total amount of profit shared since joining the platform.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/total-profit-sharing`

> Request example

```shell
GET /api/v5/copytrading/total-profit-sharing

```

#### Request Parameters
| Parameter  | Type   | Required | Description                                                                 |
| :------ | :----- |:-----|:----------------------------------------------------------------------------|
| instType     | String | No      | Instrument type<br>`SPOT`<br>`SWAP`<br>It returns all types by default.   |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "totalProfitSharingAmt": "0.6584928",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String   | The currency of profit sharing.                                                                                                                                                                                         |
| totalProfitSharingAmt      | String   | Total profit sharing amount.                                                                                                                                                                                       |
| instType  | String     | Instrument type |
