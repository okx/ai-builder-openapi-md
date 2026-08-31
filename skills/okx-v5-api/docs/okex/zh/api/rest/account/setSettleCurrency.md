---
site: okex
lang: zh
name: 设置结算币种
method: POST
path: /api/v5/account/set-settle-currency
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 设置结算币种 [trading-account-rest-api-set-settle-currency]

仅适用于 USD 本位合约。

#### 限速：20 次/2 秒
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/account/set-settle-currency`

> 请求示例

```shell
POST /api/v5/account/set-settle-currency
body
{
    "settleCcy": "USDC"
}

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                     |
| :------ | :----- | :------- |:-----------------------------------------------------------------------|
| settleCcy | String | 是      | USD 本位合约结算币种                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data" :[
          {
            "settleCcy":"USDC"
          }
    ]  
}
```

#### 返回参数

| 参数名  | 类型   | 描述   |
| :------ | :----- |:-----|
| settleCcy   | String | USD 本位合约结算币种 |
