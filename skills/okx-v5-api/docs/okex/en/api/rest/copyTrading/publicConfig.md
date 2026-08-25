---
site: okex
lang: en
name: GET / Copy trading configuration
method: GET
path: /api/v5/copytrading/public-config
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Copy trading configuration

Public endpoint. Retrieve copy trading parameter configuration information of copy settings

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-config`

> Request example

```shell
GET /api/v5/copytrading/public-config?instType=SWAP

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                   |
| :------ | :----- |:---------|:----------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                 |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "maxCopyAmt": "1000",
            "maxCopyRatio": "100",
            "maxCopyTotalAmt": "30000",
            "maxSlRatio": "0.75",
            "maxTpRatio": "1.5",
            "minCopyAmt": "20",
            "minCopyRatio": "0.01"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**         | **Type** | **Description**                                                                                    |
|:----------------------| :------- |:---------------------------------------------------------------------------------------------------|
| maxCopyAmt          | String | Maximum copy amount per order in USDT when you are using copy mode `fixed_amount`                  |
| minCopyAmt                   | String | Minimum copy amount per order in USDT when you are using copy mode `fixed_amount`                  |
| maxCopyTotalAmt      | String  | Maximum copy total amount under the certain lead trader, the minimum is the same with `minCopyAmt` |
| minCopyRatio | String  | Minimum ratio per order when you are using copy mode `ratio_copy`                                  |
| maxCopyRatio           | String  | Maximum ratio per order when you are using copy mode `ratio_copy`                                  |
| maxTpRatio           | String  | Maximum ratio of taking profit per order, the minimum is 0                                         |
| maxSlRatio           | String  | Maximum ratio of stopping loss per order, the minimum is 0                                         |
