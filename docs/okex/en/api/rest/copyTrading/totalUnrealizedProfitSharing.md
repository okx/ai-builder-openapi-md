---
site: okex
lang: en
name: GET / Total unrealized profit sharing
method: GET
path: /api/v5/copytrading/total-unrealized-profit-sharing
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Total unrealized profit sharing

The leading trader gets the total unrealized amount of profit shared.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/total-unrealized-profit-sharing`

> Request example

```shell
GET /api/v5/copytrading/total-unrealized-profit-sharing

```

#### Request Parameters
| Parameter  | Type   | Required | Description                                   |
| :------ | :----- |:-----|:----------------------------------------------|
| instType     | String | No      | Instrument type<br>`SWAP`, the default value. |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "profitSharingTs": "1705852800000",
            "totalUnrealizedProfitSharingAmt": "0.114402985553185"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                                                              |
| :---------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------|
| profitSharingTs      | String   | The settlement time for the total unrealized profit sharing amount. Unix timestamp format in milliseconds, e.g.1597026383085 |
| totalUnrealizedProfitSharingAmt      | String   | Total unrealized profit sharing amount                                                                                       |
