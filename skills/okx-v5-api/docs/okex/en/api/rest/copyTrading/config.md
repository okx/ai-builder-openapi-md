---
site: okex
lang: en
name: GET / Account configuration
method: GET
path: /api/v5/copytrading/config
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Account configuration

Retrieve current account configuration related to copy/lead trading.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP request

`GET /api/v5/copytrading/config`

> Request example

```shell
GET /api/v5/copytrading/config

```

#### Request Parameters

None

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "copyTraderNum": "1",
                    "instType": "SWAP",
                    "maxCopyTraderNum": "100",
                    "profitSharingRatio": "0",
                    "roleType": "1"
                },
                {
                    "copyTraderNum": "",
                    "instType": "SPOT",
                    "maxCopyTraderNum": "",
                    "profitSharingRatio": "",
                    "roleType": "0"
                }
            ],
            "nickName": "155***9957",
            "portLink": "",
            "uniqueCode": "5506D3681454A304"
        }
    ],
    "msg": ""
}

```

#### Response parameters

| **Parameter**        | **Type** | **Description**                                                                                 |
|:---------------------|:---------|:------------------------------------------------------------------------------------------------|
| uniqueCode           | String   | User unique code                                                                                |
| nickName             | String   | Nickname                                                                                        |
| portLink             | String   | Portrait link                                                                                   |
| details              | Array of objects    | Details                                                                                         |
| > instType           | String   | Instrument type<br>`SPOT`<br>`SWAP`                                                            |
| > roleType           | String   | Role type<br>`0`: General user<br>`1`: Leading trader<br>`2`: Copy trader                      |
| > profitSharingRatio | String   | Profit sharing ratio. <br>Only applicable to lead trader, or it will be "". 0.1 represents 10% |
| > maxCopyTraderNum   | String   | Maximum number of copy traders |
| > copyTraderNum      | String   | Current number of copy traders |
