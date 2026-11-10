---
site: us
lang: en
name: Get collateral assets
method: GET
path: /api/v5/account/collateral-assets
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get collateral assets

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/collateral-assets`

> Request Example

```shell
GET /api/v5/account/collateral-assets
```

#### Request Parameters

| **Parameters**    | **Types**  | **Required**   | **Description**                                                                                         |
|:------------------|:-----------|:---------------|:--------------------------------------------------------------------------------------------------------|
| ccy               | String     | No             | Single currency or multiple currencies (no more than 20) separated with comma, e.g. "BTC" or "BTC,ETH". |
| collateralEnabled | Boolean    | No             | Whether or not to be a collateral asset                                                                 |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data" :[
          {
            "ccy":"BTC",
            "collateralEnabled": true
          },
          {
            "ccy":"ETH",
            "collateralEnabled": false
          }
    ]  
}
```

#### Response Parameters

| **Parameter**       | **Type** | **Description**                         |
|:--------------------|:---------|:----------------------------------------|
| ccy               | String     | Currency, e.g. `BTC`                    |
| collateralEnabled | Boolean    | Whether or not to be a collateral asset |
