---
site: okex
lang: en
name: GET / Unrealized profit sharing details
method: GET
path: /api/v5/copytrading/unrealized-profit-sharing-details
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Unrealized profit sharing details

The leading trader gets the profit sharing details that are expected to be shared in the next settlement cycle.<br>
The unrealized profit sharing details will update once there copy position is closed.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/unrealized-profit-sharing-details`

> Request example

```shell
GET /api/v5/copytrading/unrealized-profit-sharing-details

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
            "nickName": "Potato",
            "portLink": "",
            "ts": "1669901824779",
            "unrealizedProfitSharingAmt": "0.455472",
            "instType": "SWAP"
        },
        {
            "ccy": "USDT",
            "nickName": "Apple",
            "portLink": "",
            "ts": "1669460210113",
            "unrealizedProfitSharingAmt": "0.033608",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String   | The currency of profit sharing. e.g. USDT                                                                                                                                                                                         |
| unrealizedProfitSharingAmt      | String   | Unrealized profit sharing amount.                                                                                                                                                                                      |
| nickName      | String   | Nickname of copy trader.                                                                                                                                                                                       |
| instType  | String     | Instrument type |
| portLink     | String    | Portrait link |
| ts      | String   | Update time.                                                                                                                                                                                       |
