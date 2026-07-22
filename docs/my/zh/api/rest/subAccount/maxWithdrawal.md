---
site: my
lang: zh
name: 获取子账户最大可转余额
method: GET
path: /api/v5/account/subaccount/max-withdrawal
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取子账户最大可转余额 [sub-account-rest-api-get-sub-account-maximum-withdrawals]

获取子账户最大可转余额（适用于母账户）。不指定币种会返回所有拥有的币种资产可划转数量。

#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/account/subaccount/max-withdrawal`

> 请求示例

```shell
GET /api/v5/account/subaccount/max-withdrawal?subAcct=test1
  
```

#### 请求参数

| 参数名     | 类型     | 是否必须 | 描述                                         |
|---------|--------|------|--------------------------------------------|
| subAcct | String | 是    | 子账户名称                                      |
| ccy     | String | 否    | 币种，如 `BTC` <br>支持多币种查询（不超过20个），币种之间半角逗号分隔 |

> 返回结果

```json
{
   "code":"0",
   "data":[
      {
         "ccy":"BTC",
         "maxWd":"3",
         "maxWdEx":"",
         "spotOffsetMaxWd":"3",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"ETH",
         "maxWd":"15",
         "maxWdEx":"",
         "spotOffsetMaxWd":"15",
         "spotOffsetMaxWdEx":""
      },
      {
         "ccy":"USDT",
         "maxWd":"10600",
         "maxWdEx":"",
         "spotOffsetMaxWd":"10600",
         "spotOffsetMaxWdEx":""
      }
   ],
   "msg":""
}
```

#### Response Parameters

| 参数名               | 类型     | 描述                                 |
|-------------------|--------|------------------------------------|
| ccy               | String | 币种                                 |
| maxWd             | String | 最大可划转数量（不包含`跨币种保证金模式`借币金额）         |
| maxWdEx           | String | 最大可划转数量（包含`跨币种保证金模式`借币金额）          |
| spotOffsetMaxWd   | String | 现货对冲不支持借币最大可转数量 <br>仅适用于`组合保证金模式` |
| spotOffsetMaxWdEx | String | 现货对冲支持借币的最大可转数量 <br>仅适用于`组合保证金模式` |
