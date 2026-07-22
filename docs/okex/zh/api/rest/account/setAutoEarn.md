---
site: okex
lang: zh
name: 设置自动赚币
method: POST
path: /api/v5/account/set-auto-earn
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 设置自动赚币 [trading-account-rest-api-set-auto-earn]

开启/关闭自动赚币

#### 限速：2次/2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/set-auto-earn`

> 请求示例

```shell
// 开启自动赚币
{
   "earnType": "0",
   "ccy":"BTC",
   "action":"turn_on"
}

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| earnType | String | 否 | 自动赚币类型<br>`0`: 自动赚币 (自动出借、自动质押) <br>`1`: 自动赚币（USDG 赚币）<br> 默认值为 `0` |
| ccy | String | 是 | 币种 |
| action | String | 是 | 自动赚币操作类型<br>`turn_on`: 开启自动赚币<br>`turn_off`: 关闭自动赚币<br>~~`amend`: 修改最低年化收益率，仅适用于 earnType `0`~~（已弃用） |
| apr | String | 可选 | ~~最低年化收益率~~（已弃用） |

> 返回结果

```json
{
   "code":"0",
   "msg":"",
   "data":[
      {
         "earnType": "0",
         "ccy":"BTC",
         "action":"turn_on",
         "apr":"0.01"
      }
   ]
}

```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| earnType | String | 自动赚币类型<br>`0`: 自动赚币 (自动出借、自动质押) <br>`1`: 自动赚币（USDG 赚币）|
| ccy | String | 币种 |
| action | Boolean | 自动赚币操作类型<br> `turn_on`<br> `turn_off`<br> ~~`amend`~~（已弃用） |
| apr | String | ~~最低年化收益率~~（已弃用） |
