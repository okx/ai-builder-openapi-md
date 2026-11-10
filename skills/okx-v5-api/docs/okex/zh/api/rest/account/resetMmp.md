---
site: okex
lang: zh
name: 重置 MMP 状态
method: POST
path: /api/v5/account/mmp-reset
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 重置 MMP 状态 [trading-account-rest-api-reset-mmp-status]

一旦 MMP 被触发，可以使用该接口解冻。<br>
仅适用于组合保证金账户模式下的期权订单，且有 MMP 权限。

<aside class="notice">
在模拟盘环境中，MMP 配置可能会被系统定期重置。若您的模拟盘 MMP 状态被意外重置，请联系您的客户经理或发邮件至 institutional@okx.com。
</aside>

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/mmp-reset`

> 请求示例

```shell
POST /api/v5/account/mmp-reset
body
{
    "instType":"OPTION",
    "instFamily":"BTC-USD"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                                |
| :------ | :----- |:-----|:------------------------------------------------------------------------------------------------------------------|
| instType  | String | 否    | 交易产品类型<br>`OPTION`:期权<br>默认为期权                                                                                    |
| instFamily  | String | 是    | 交易品种                                                                       |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "result":true
        }
    ]
}
```

#### 返回参数

| 参数名 | 类型   | 描述                                              |
|:----| :----- |:------------------------------------------------|
| result    | Boolean | 重置结果<br>`true`:将做市商保护状态重置为了 inactive 状态<br>false：重置失败   |
