---
site: okex
lang: zh
name: 设置质押币种
method: POST
path: /api/v5/account/set-collateral-assets
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 设置质押币种 [trading-account-rest-api-set-collateral-assets]

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-collateral-assets`

> 请求示例

```shell
# 设置全部币种为可质押资产
POST /api/v5/account/set-collateral-assets
body
{
    "type":"all",
    "collateralEnabled":true
}

# 设置自定义不可质押资产
POST /api/v5/account/set-collateral-assets
body
{
    "type":"custom",
    "ccyList":["BTC","ETH"],
    "collateralEnabled":false
}

```

#### 请求参数

| 参数名               | 类型              | 是否必须    | 描述                                                |
|:------------------|:----------------|:--------|:--------------------------------------------------|
| type              | String          | 是       | 设置币种类型<br>`all`：全部<br>`custom`：自定义                |
| collateralEnabled | Boolean          | 是       | 是否设置为质押币种<br>`true`：设置为质押币<br>`false`：取消质押币的设置    |
| ccyList           | Array of strings | 可选      | 币种列表，如 ["BTC","ETH"]<br>当type=`custom`,该字段必传。     |

> 返回结果

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

#### 返回参数

| 参数名                 | 类型                | 描述                                              |
|:--------------------|:------------------|:------------------------------------------------|
| type                | String            | 设置币种类型<br>`all`：全部<br>`custom`：自定义              |
| collateralEnabled   | Boolean           | 是否已设置为质押币种<br>`true`：设置为质押币<br>`false`：取消质押币的设置 |
| ccyList             | Array of strings   | 币种列表，如 ["BTC","ETH"]                            |
