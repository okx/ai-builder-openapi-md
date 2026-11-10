---
site: okex
lang: zh
name: 设置现货对冲占用
method: POST
path: /api/v5/account/set-riskOffset-amt
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置现货对冲占用 [trading-account-rest-api-set-risk-offset-amount]
用户自定义现货对冲占用数量，不代表实际现货对冲占用数量。仅适用于组合保证金模式。

#### 限速：10次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-riskOffset-amt`

> 请求示例

```shell
# 设置现货对冲占用
POST /api/v5/account/set-riskOffset-amt
{
   "ccy": "BTC",
   "clSpotInUseAmt": "0.5"
}

```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述         |
| --- | --- | --- |------------|
| ccy | String | 是 | 币种，如 `BTC`   |
| clSpotInUseAmt | String | 是 | 用户自定义现货对冲数量 |

> 返回示例

```json
{
   "code": "0",
   "msg": "",
   "data": [
      {
         "ccy": "BTC",
         "clSpotInUseAmt": "0.5"
      }
   ]
}
```

#### 返回参数

| 参数名 | 类型 | 描述          |
| --- | --- |-------------|
| ccy | String | 币种，如 `BTC`    |
| clSpotInUseAmt | String | 用户自定义现货对冲数量 |
