---
site: okex
lang: en
name: Set collateral assets
method: POST
path: /api/v5/account/set-collateral-assets
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set collateral assets

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-collateral-assets`

> Request Example

```shell
# Set all assets to be collateral
POST /api/v5/account/set-collateral-assets
body
{
    "type":"all",
    "collateralEnabled":true
}

# Set custom assets to be non-collateral
POST /api/v5/account/set-collateral-assets
body
{
    "type":"custom",
    "ccyList":["BTC","ETH"],
    "collateralEnabled":false
}
```

#### Request Parameters

| Parameter     | Type              | Required    | Description                                                                                                         |
| :--------- |:------------------|:------------|:--------------------------------------------------------------------------------------------------------------------|
| type              | String            | true        | Type<br>`all`<br>`custom`                                                                                           |
| collateralEnabled | Boolean           | true        | Whether or not set the assets to be collateral<br>`true`: Set to be collateral<br>`false`: Set to be non-collateral |
| ccyList           | Array of strings   | conditional | Currency list, e.g. ["BTC","ETH"]<br>If type=`custom`, the parameter is required.                                   |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data" :[
      {
        "type":"all",
        "ccyList":["BTC","ETH"],
        "collateralEnabled":false
      }
    ]  
}
```

#### Response Parameters

| Parameter     | Type   | Description                                                                                                                               |
| :--------- | :----- |:------------------------------------------------------------------------------------------------------------------------------------------|
| type              | String            | Type<br>`all`<br>`custom`                                                                                                                 |
| collateralEnabled | Boolean           | Whether or not set the assets to be collateral<br>`true`: Set to be collateral<br>`false`: Set to be non-collateral                       |
| ccyList           | Array of strings   | Currency list, e.g. ["BTC","ETH"]                                                    |
