---
site: okex
lang: zh
name: 查看质押币种
method: GET
path: /api/v5/account/collateral-assets
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看质押币种 [trading-account-rest-api-get-collateral-assets]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/collateral-assets`

> 请求示例

```shell
GET /api/v5/account/collateral-assets
```

#### 请求参数

| 参数名               | 类型   | 是否必须 | 描述                                           |
|:------------------| :----- |:-----|:---------------------------------------------|
| ccy               | String | 否    | 币种<br>支持多币种查询（不超过20个），币种之间半角逗号分隔，如 "BTC,ETH" |
| collateralEnabled | Boolean | 否    | 是否为质押币                                       |

> 返回结果

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

#### 返回参数

| **参数名**             | **类型**  | **描述**     |
|:--------------------|:--------|:-----------|
| ccy                 | String | 币种，如 `BTC` |
| collateralEnabled   | Boolean | 是否为质押币     |
